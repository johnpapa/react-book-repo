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
- add the following to `package.json` and the `scripts` tag: "storybook": "start-storybook -p 9009 -s public"

The `.storybook` directory contains the following:
- addons.js
- config.js
`config.js` tells `storybook` where our *stories* can be found.
The `stories` directory contains an `index.js` and *stories* which is something that storybook picks up and renders as a HTML page. 

The `/stories/index.js` looks like this:
```js
import React from 'react';

import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';
import { linkTo } from '@storybook/addon-links';

import { Button, Welcome } from '@storybook/react/demo';

storiesOf('Welcome', module).add('to Storybook', () => <Welcome showApp={linkTo('Button')} />);

storiesOf('Button', module)
  .add('with text', () => <Button onClick={action('clicked')}>Hello Button</Button>)
  .add('with some emoji', () => (
    <Button onClick={action('clicked')}>
      <span role="img" aria-label="so cool">
        😀 😎 👍 💯
      </span>
    </Button>
  ));
```

## Run
By running
```
yarn storybook 
```
It will startup story book and we can navigate to `http://localhost:9000`. It will look like this:

![](/assets/Screen Shot 2018-06-06 at 14.23.34.png)
## Add a story
## Improving - Dedicated story folders
