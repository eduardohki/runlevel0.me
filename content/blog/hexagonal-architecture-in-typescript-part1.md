+++
title = "Hexagonal Architecture in TypeScript, Part 1"
date = "2024-02-29T15:08:51+01:00"

categories = ['Software Engineering']
series = ['Hexagonal Architecture in TypeScript']
tags = ['DDD', 'TDD', 'TypeScript']
+++

## Introduction

Since I started calling myself a developer a few years ago, I've been trying to understand how to best design software so that it fits in one's head and stays easy to change over time. In the meantime, I have heard of things like Domain-Driven Design (DDD), Behavior-Driven Development (BDD), Test-Driven Development (TDD), and Clean/Onion/Hexagonal Architecture, but it was never easy for me to get it, nor had I someone showing me how those things work.

One of the biggest challenges for me was that usually these practices come from the world of object-oriented languages like Java and C#, and many of the examples I've come across conflate these concepts and practices with object-oriented ones, sometimes even misleading one away from what it really is about (like assuming TDD requires mocks and testing each and every implementation detail). It all seemed over-complicated for me, and my gut feeling told me this was not a *skill* issue.

Eventually I came across websites like [Dojo Consortium](https://dojoconsortium.org/docs/work-decomposition/) and [MinimumCD](https://minimumcd.org/minimumcd/), which allowed me to understand how and for what purpose these practices fit together. What remained for me was to understand how to implement them in practice and in the code, and I wanted to do this by applying [functional thinking](https://www.manning.com/books/grokking-simplicity).

It was after coming across [this](https://alexis-lozano.com/hexagonal-architecture-in-rust-1/) excellent series of articles by *Alex Lozano* on Hexagonal Architecture in Rust, that the final pieces fell into place.

My aim in this series of posts is to present an example of how to apply DDD, TDD and Hexagonal Architecture in the context of an application written in TypeScript. Without further ado, let's go!

## Hexagonal Architecture

According to the following quote from the author's [website](https://alistair.cockburn.us/hexagonal-architecture/), the goal of Hexagonal Architecture is to

> Create your application to work without either a UI or a database so you can run automated regression-tests against the application, work when the database becomes unavailable, and link applications together without any user involvement.

In other words, it is about *decoupling* the core application from all external dependencies and underlying infrastructure, so that its use-cases can be easily tested locally in your laptop, for example.

Here enters TDD: think about the ability to quickly test the whole application *before* committing your changes. Hexagonal Architecture optimizes for that. It allows you to have fearless control over of your code. This requires defining the features/use-cases before even starting to code.

## Did I mention BDD?

For this series I'm conceiving a fictional team building a Project Collaboration Platform from scratch. We neither know nor care where it will run, which database to use and so on.

We do, however, have a defined set of use-cases to start building:

```feature
Feature: As a Project Manager, I want to manage new and existing Projects

  Scenario: Creating a new Project
    Given I am logged in
    When I select to create a new Project
    And I give the name of the project
    Then a new Project gets created
    And I get redirected to the new Project

  Scenario: Changing an existing Project
    Given I am logged in
    And I have a Project
    When I select to rename an existing Project
    And I give the new Project name
    Then the Project name gets updated

  Scenario: Deleting an existing Project
    Given I am logged in
    And I have a Project
    When I select to delete an existing Project
    Then the Projects gets deleted
```

For now we're going to focus on the first scenario/use-case: *Creating a new Project*.

One may well argue that this is a simple *CRUD* app, and that all this effort is not necessary. But bear on with me.

## Starting the first use-case with TDD

From the use-case specification we can derive a few things:

* For now a *Project* only needs a name
* The *Project* will need a unique identifier, which in this case we'll use `UUID`s

Here I can think of a few behaviors we want this use-case to cover:

1. The ID of the project should be a valid UUID
1. The name of the project cannot be empty of null
1. The name of the project shouldn't be too long
1. It should be able to create a valid project

Let's start the code by initializing a new TypeScript project:

```zsh
mkdir project-collab-platform && cd project-collab-platform
npm i typescript --save-dev
npx tsc --init
```

And then create our first files related to our use-case:

```sh
src/
└── domain/
    ├── createProject.spec.ts
    ├── createProject.ts
    └── entities.ts
```

Since we want to start with the tests, we'll install [vitest](https://vitest.dev/):

```sh
npm install -D vitest
```

We also will add the following section in our `package.json`:

```json
{
  "scripts": {
    "test": "vitest"
  }
}
```

And start our test runner in a terminal window:

```sh
npm test
```

Then, finally, we'll write our first test case:

```ts
// src/domain/createProject.spec.ts
import { describe, expect, it } from "vitest";
import { createProject } from "./createProject";

describe("createProject", () => {
	it("should return an error if the ID format is invalid", () => {
		expect(() => {
			createProject("an invalid id", "my project name");
		}).toThrowError(/Project ID is not an UUID/);
	});
});
```

It should fail, as we don't have any code written yet. Let's fix that:

```ts
// src/domain/entities.ts
export type Project = {
	id: string;
	name: string;
};
```

```ts
// src/domain/createProject.ts
import { validate as isValidUuid } from "uuid";
import { Project } from "./entities";

export const createProject = (id: string, name: string): Project => {
	if (!isValidUuid(id)) {
		throw new Error("Project ID is not an UUID");
	}
};
```

Let's not forget to install the `uuid` package:

```sh
npm install --save uuid
npm install --save-dev @types/uuid
```

The test should now pass. Let's implement another test:

```ts
// src/domain/createProject.spec.ts
import { describe, expect, it } from "vitest";
import { createProject } from "./createProject";

describe("createProject", () => {
	...
    it("should not allow empty project name", () => {
		expect(() => {
			createProject("9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2", "");
		}).toThrowError(/Project name must contain at least 1 character/);
	});
});
```

The test should fail. Let's do the implementation:

```ts
// src/domain/createProject.ts
import { validate as isValidUuid } from "uuid";
import { Project } from "./entities";

export const createProject = (id: string, name: string): Project => {
	if (!isValidUuid(id)) {
		throw new Error("Project ID is not an UUID");
	}
	if (name.length === 0) {
		throw new Error("Project name must contain at least 1 character");
	}
};
```

Tests should pass. Let's now check for max Project name length:


```ts
// src/domain/createProject.spec.ts
import { describe, expect, it } from "vitest";
import { createProject } from "./createProject";

describe("createProject", () => {
    ...
	it("should not allow project name with more than 50 characters", () => {
		expect(() => {
			createProject(
				"9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2",
				"This project name is too long, it should definitely fail",
			);
		}).toThrowError(/Project name must contain at most 50 characters/);
	});
});
```

Again the tests should fail. If yes, we go for implementing it:

```ts
// src/domain/createProject.ts
import { validate as isValidUuid } from "uuid";
import { Project } from "./entities";

export const createProject = (id: string, name: string): Project => {
	if (!isValidUuid(id)) {
		throw new Error("Project ID is not an UUID");
	}
	if (name.length === 0) {
		throw new Error("Project name must contain at least 1 character");
	}
	if (name.length > 50) {
		throw new Error("Project name must contain at most 50 characters");
	}
};
```

At last, after the tests passing, we test if it returns a valid Project:

```ts
// src/domain/createProject.spec.ts
import { describe, expect, it } from "vitest";
import { createProject } from "./createProject";

describe("createProject", () => {
    ...
	it("should create a new project with the given id and name", () => {
		expect(
			createProject("9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2", "my project"),
		).toStrictEqual({
			id: "9fa98a0a-c2a5-4f1e-bcf9-0a60ec7a65c2",
			name: "my project",
		});
	});
});
```

Again tests fail, allowing us to implement it then:

```ts
// src/domain/createProject.ts
import { validate as isValidUuid } from "uuid";
import { Project } from "./entities";

export const createProject = (id: string, name: string): Project => {
	if (!isValidUuid(id)) {
		throw new Error("Project ID is not an UUID");
	}
	if (name.length === 0) {
		throw new Error("Project name must contain at least 1 character");
	}
	if (name.length > 50) {
		throw new Error("Project name must contain at most 50 characters");
	}
	return { id, name };
};
```

Now our first scenario/use-case is implemented with all tests passing. If you look closely, you'll notice that these tests are effectively a runnable specification.

The code written here can be found on [GitHub](https://github.com/eduardohki/project-collab-platform/tree/part-1).

## What's next?

So far we implemented our first use-case using TDD, but the case for Hexagonal Architecture is not clear yet. In the next article in this series we'll see how to implement a repository to store the *Projects* created in this use-case. The repository will be defined by an interface (or type, in case of TypeScript) and an in-memory implementation will be created for use in tests.

See you in the next article!
