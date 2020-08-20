# with-flickr

A Flickr gallery app for With.

## Devlog

### Day 1 (initial bootstrapping, problem exploration)

#### Initial setup (5min)

- Bootstrapped with create-react-app and the Redux+Typescript template (just in case Redux is useful here, helps to skip the boilerplate)
- Pulling in Material-UI as a baseline component library (my goto)

#### Connecting to Flickr (30min)

The Flickr docs were a bit strange to navigate at first. I created a set of API credentials and noticed a secret was included. Obviously I don't want to include that in my React client, but it took a bit of digging to confirm my suspicion that only the key would be required for client-side API usage. Glad I don't have to create a backend to proxy the API.

Flickr's API also has some... interesting aspects to it. Worried me at first that I'd have to parse XML, but I did find the `format=json` parameter. What was curious is that they seem to wrap their JSON in some kind of 'callback' syntax which makes the response invalid JSON by default, unless you pass another parameter to remove that.

With at least the ability to fetch JSON sorted out, though, I opted for a standard fetch to interact with the API to keep things simple (had it been XML only, for instance, I might have considered utilizing a community-built SDK if it was available - XML can be tricky to parse correctly on the client since structures like arrays are often left up to inference).

#### Redux, and async actions (20min)

I ended up utilizing Redux as a state store here, although the app is still small. It's convenient to share the search information between different components, and it allows me to centralize the logic which triggers the API search effect when the search term changes.

For async behavior, I opted for thunk actions. They're tried and true, and pretty simple to use. I've also used redux-saga in the past, which is an excellent way to model complex side-effects in a testable pattern. It's more suited to larger-scale apps, though, and the team has to be ok with generators.

My code is very similar to the Redux template boilerplate. I'm ok with that. Thunks and bog-standard Redux may not be flashy, they may not even be my first choice of tools, but they're ubiquitous, simple, and they get the job done. These qualities are very valuable both for scaling the codebase and team size later on.

#### Wrapping up the first hour

At this point, I had a basic (if ugly) starting point with a React app which can filter Flickr images based on a search term.

Todo:

1. Pagination
2. Arranging items in a responsive grid
3. Polishing the experience
4. Lightboxing of individual items?
5. More?

### Day 2 (feature completion)

#### Rounding out functionality (1h)

Having spent an hour with the codebase initially just building a foundation of interacting with the Flickr API, my focus for the next hour or so was filling out the remaining baseline features - namely pagination and the grid layout.

Pagination is relatively trivial. For a simple initial pass I opted to include a 'Show more' button at the bottom of the photo list which fetches the next page of data from the API and appends it to the current result set. This button only appears if there is a next page to show - since the API is kind enough to provide a total page count. The final detail is to ensure the result set is reset upon a new search, so we don't just keep appending.

As for the grid - CSS grid makes it pretty trivial. I opted for a set of breakpoint-based cell sizes which seemed effective when I tested through the browser's responsive design tools.

#### Polish!

I gotta admit I just want an excuse to do some animation. I had the chance to play around just a little bit with Framer Motion a while back, and I thought this would be a great time to dive back in. It's just a delightfully simple and powerful library with a few little features that I think give it an edge over the incumbent `react-spring`.

My visual goals for the app:

1. Create a nice transition from search to results and delay the current loading state to only appear if it takes a while. Using animation to convey loading is a better experience than a spinner.
2. Stagger the animation of grid children into the grid as they're loaded
3. Have some fun with subtle interactivity
4. Find a nice color palette

## Development / Setup

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app), using the [Redux](https://redux.js.org/) and [Redux Toolkit](https://redux-toolkit.js.org/) template.

### Available Scripts

In the project directory, you can run:

#### `npm start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

#### `npm test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

#### `npm run build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

#### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

### Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).
