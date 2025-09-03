# RedwoodSDK Drizzle Addon

This addon adds Drizzle to your RedwoodSDK project.

## How to add it to your project

These instructions assume you are starting with a RedwoodSDK project, for example from `npx create-rwsdk -t minimal my-project-name`.

### 0. Decide whether to add this manually or via AI

To use your editor's AI agent support to add this addon for you (e.g. Cursor, VSCode Copilot):

1. Make sure your project is open in your editor. You can create a new project with: `npx create-rwsdk -t minimal my-project-name`.
2. Open a new AI chat window for your project
3. Make sure you are in Agent mode
4. Send the following chat prompt in the chat window - it will do the rest for you!

```
Please run the following and then follow the install instructions:

curl -s https://raw.githubusercontent.com/ahaywood/github-stars-addon/refs/heads/main/README.md -o /tmp/github-stars-addon-readme.md && cat /tmp/github-stars-addon-readme.md

Use these instructions to install the add-on in my RedwoodSDK project.
```

Alternatively, to apply this addon manually, simply follow the steps below.

## 1. Within the `src/app` directory, create a `lib` folder. Inside, create a file called `githubStars.ts`

Add the following contents to the new file:

```ts
export const getGithubStars = async () => {
  try {
    const response = await fetch("https://api.github.com/repos/redwoodjs/sdk", {
      headers: {
        Accept: "application/vnd.github.v3+json",
        "User-Agent": "RedwoodJS-Docs",
      },
      cf: {
        cacheTtl: 14400, // Cache for 4 hours
        cacheEverything: true,
      },
    });

    if (!response.ok) {
      console.error(
        `GitHub API error: ${response.status} ${response.statusText}`
      );
      return; // fallback to hardcoded value
    }

    const data = (await response.json()) as { stargazers_count: number };
    return data.stargazers_count;
  } catch (error) {
    console.error("Error fetching GitHub stars:", error);
    return; // fallback to hardcoded value
  }
};
```

In this case, we're getting the number of stars from the `redwoodjs/sdk` repo, as defined on line 3:

```ts
const response = await fetch("https://api.github.com/repos/redwoodjs/sdk", {
```

You'll need to swap this out of the repo you're trying to access.

On line 9, we're caching the result for 4 hours:

```ts
cacheTtl: 14400, // Cache for 4 hours
```

The user may want to shorten or lengthen the cache, depending on how frequently this number needs to be updated and how many users are accessing the application.

---

Now, the number of GitHub stars can be accessed on any page, by importing `gitHubStars`:

```tsx
import { getGithubStars } from "@/app/lib/githubStars";
```

and calling the function:

```tsx
{getGithubStars()}
```