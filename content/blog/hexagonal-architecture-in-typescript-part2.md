+++
title = "Hexagonal Architecture in TypeScript, Part 2"
date = "2024-03-05T15:54:01+01:00"

categories = ['Software Engineering']
series = ['Hexagonal Architecture in TypeScript']
tags = ['BDD', 'TDD', 'TypeScript']
+++

This article is comprised of multiple parts:

* [Part 1: Introduction and First Use-Case]({{< ref "hexagonal-architecture-in-typescript-part1.md" >}})
* Part 2: In-Memory Repository

## Introduction

In the previous post we implemented the first scenario/use-case of our fictional Project Collaboration Platform -- `createProject` -- using Test-Driven Development (TDD) from a Behavior-Driven Development (BDD) specification.

Although it was useful for establishing a runnable specification about how a Project should look like after creation, the program still doesn't store it or do anything with it.

## Enter the In-Memory Repository

The idea of building an In-Memory Repository in the context of Hexagonal Architecture is twofold: (1) to allow us to test the behavior or flow of the entire system decoupled from its external dependencies -- while always ensuring fast feedback -- and (2) effectively decoupling the Repository implementation from the core application, so it doesn't matter which implementation is used behind the scenes.

Another advantage is that the Repository interface in question is defined and evolved via TDD alongside the core application.

## Implementing it in the first use-case

To get started, we're going to create a new file (`src/repositories/project.ts`) in our folder structure:

```sh
src/
├── domain/
│   ├── createProject.spec.ts
│   ├── createProject.ts
│   └── entities.ts
└── repositories/
    └── project.ts
```

In TDD, it is important to understand that tests are intended as measurements used to reduce the risk of breaking the existing system behavior when implementing changes. It therefore implies that if we change both the code *and* the tests at the same time, the change in question becomes risky.

However, in this instance this is unavoidable, since we will have to change the signature of the function under test and initialize the In-Memory Repository in the tests themselves. That is also why we are doing this at this initial point, with only one scenario/use-case implemented so far.

Let's start by creating a new interface for the Repository, containing an `insert` function:

```ts
// src/repositories/project.ts
import { Project } from "../domain/entities";

export type ProjectRepository = {
	insert(input: Project): Project;
};
```

And then adding it to the existing use-case:

```ts
// src/domain/createProject.ts
import { validate as isValidUuid } from "uuid";
import { Project } from "./entities";
import { ProjectRepository } from "../repositories/project";

export const createProject = (
	repo: ProjectRepository,
	id: string,
	name: string,
): Project => {
	if (!isValidUuid(id)) {
		throw new Error("Project ID is not an UUID");
	}
	if (name.length === 0) {
		throw new Error("Project name must contain at least 1 character");
	}
	if (name.length > 50) {
		throw new Error("Project name must contain at most 50 characters");
	}
	return repo.insert({ id, name });
};
```

If we run the tests at this point they are going to fail because the function signature changed. To be able to test in the first place, we need first to create the In-Memory Repository. Let's do it:

```ts
// src/repositories/project.ts
import { Project } from "../domain/entities";

export type ProjectRepository = {
	insert(input: Project): Project;
};

export class InMemoryProjectRepository implements ProjectRepository {
	insert = (input: Project): Project => {
		return input;
	};
}
```

Note that it is still not actually storing anything. This is intentional. Now let's update the tests to allow for testing the use-case with the In-Memory Repository:

```ts
// src/domain/createProject.spec.ts
import { describe, expect, it } from "vitest";
import { createProject } from "./createProject";
import { InMemoryProjectRepository } from "../repositories/project";

describe("createProject", () => {
	it("should return an error if the ID format is invalid", () => {
		const repo = new InMemoryProjectRepository();
		expect(() => {
			createProject(repo, "an invalid id", "my project name");
		}).toThrowError(/Project ID is not an UUID/);
	});
	it("should not allow empty project name", () => {
		const repo = new InMemoryProjectRepository();
		expect(() => {
			createProject(repo, "9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2", "");
		}).toThrowError(/Project name must contain at least 1 character/);
	});
	it("should not allow project name with more than 50 characters", () => {
		const repo = new InMemoryProjectRepository();
		expect(() => {
			createProject(
				repo,
				"9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2",
				"This project name is too long, it should definitely fail",
			);
		}).toThrowError(/Project name must contain at most 50 characters/);
	});
	it("should create a new project with the given id and name", () => {
		const repo = new InMemoryProjectRepository();
		expect(
			createProject(repo, "9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2", "my project"),
		).toStrictEqual({
			id: "9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2",
			name: "my project",
		});
	});
});
```

Each test now initializes our In-Memory Repository and passes it to the function under test as a parameter. Now the tests should pass again.

From this point onward we should be again able to change either the test or the code (*Phew!*).

## Adding one last missing behavior

If you look at the first post in this series, you'll see that we thought about a few behaviors that we wanted this scenario/use-case to cover:

1. The ID of the project should be a valid UUID
1. The name of the project cannot be empty of null
1. The name of the project shouldn't be too long
1. It should be able to create a valid project

There's one thing missing here: regardless if we are using `UUID`s, a Project ID should be unique. Now that we have a Repository, we can add a test for that:

```ts
// src/domain/createProject.spec.ts
import { describe, expect, it } from "vitest";
import { createProject } from "./createProject";
import { InMemoryProjectRepository } from "../repositories/project";

describe("createProject", () => {
    ...
	it("should not create a project with a non-unique ID", () => {
		const repo = new InMemoryProjectRepository();
		const projectId = "9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2";
		repo.insert({ id: projectId, name: "my project name" });
		expect(() => {
			createProject(repo, projectId, "my other project name");
		}).toThrowError(/Project ID already in use/);
	});
});
```

This test should fail, so we can now implement it:

```ts
// src/repositories/project.ts
import { Project } from "../domain/entities";

export type ProjectRepository = {
	insert(input: Project): Project;
};

export class InMemoryProjectRepository implements ProjectRepository {
	projects: Project[];

	constructor() {
		this.projects = [];
	}

	insert = (input: Project): Project => {
		if (this.projects.find((p) => p.id === input.id)) {
			throw new Error("Project ID already in use");
		}
		this.projects = [...this.projects, input];
		return input;
	};
}
```

The new test should now pass, with all tests being green.

The code written in this part 2 can be found on [GitHub](https://github.com/eduardohki/project-collab-platform/tree/part-2).

## What's next?

So far we have implemented a Repository to store the projects created in this scenario/use-case and adjusted the tests accordingly. Little by little, the general idea of Hexagonal Architecture is taking shape.

But before going for the other scenarios/use-cases, our fictional team wants to deliver this one as completely as possible, *i.e.* vertically, where the full functionality can be considered done (ignoring the login part for now). In the next article in this series, we'll create a user interface that makes this possible.

Thanks for coming along. See you in the next article!
