# Storybook
Imagine when your project grows and the number of components and developers with it. It might get hard to keep track of all the components and how they are rendered differently depending on what input they get. For those situations there exists a number of tools where you can visually display your components - a style guide. This article is about one such tool called Storybook. 

## Install & Setup
How would we install it? We need to install a CLI for this by typing:

```
npm i -g @storybook/cli
```

Once we have done so we can create our React project and from the project root we can type:

```
getstorybook
```
This will install all the needed dependencies plus:

- add a `storybook-demo/.storybook/` directory
- `src/stories`
- add the following to `package.json` and the `scripts` tag: "storybook": "start-storybook -p 9009 -s public",
