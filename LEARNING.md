# 20. React, Part II

## Stateless Components from Stateful Components

*   Child components update their parents' state
*   Child components update their *siblings*' props
*   A component should never update `this.props`
*   **A React component should use `props` to store information that can be changed, but can only be changed by a *different* component.**
*   **A React component should use `state` to store information that the component itself can change.**

```js
//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Video } from './Video';
import { Menu } from './Menu';

const VIDEOS = {
  fast: 'https://content.codecademy.com/courses/React/react_video-fast.mp4',
  slow: 'https://content.codecademy.com/courses/React/react_video-slow.mp4',
  cute: 'https://content.codecademy.com/courses/React/react_video-cute.mp4',
  eek: 'https://content.codecademy.com/courses/React/react_video-eek.mp4'
};

class App extends React.Component {
  constructor(props) {
    super(props);
    this.chooseVideo = this.chooseVideo.bind(this);
    this.state = { src: VIDEOS.fast };
  }
  
  chooseVideo(newVideo) {
    this.setState({src: VIDEOS[newVideo]})
  }

  render() {
    return (
      <div>
        <h1>Video Player</h1>
        <Menu chooseVideo={this.chooseVideo}/>
        <Video src={this.state.src}/>
      </div>
    );
  }
}

ReactDOM.render(
  <App />, 
  document.getElementById('app')
);
```

```js
//Video.js
import React from 'react';

export class Video extends React.Component {
  render() {
    return (
      <div>
        <video controls autostart autoPlay muted src={this.props.src}/>
      </div>
    );
  }
}
```

```js
//Menu.js
import React from 'react';

export class Menu extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick(e) {
    const text = e.target.value;
    this.props.chooseVideo(text);
  }
  render() {
    return (
      <form onClick={this.handleClick}>
        <input type="radio" name="src" value="fast" /> fast
        <input type="radio" name="src" value="slow" /> slow
        <input type="radio" name="src" value="cute" /> cute
        <input type="radio" name="src" value="eek" /> eek
      </form>
    );
  }
}
```

