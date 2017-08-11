React Packery Component
=======================

[![npm version](https://badge.fury.io/js/react-packery-component.svg)](http://badge.fury.io/js/react-packery-component)

#### Introduction:
A [React.js](https://facebook.github.io/react/) [Packery](http://packery.metafizzy.co/) component. (Also available as a [mixin](https://github.com/eiriklv/react-packery-mixin) if needed)

#### Support
React >= 0.14.x

#### Live demo:
[hearsay.me](http://www.hearsay.me)

#### Installation:
` $ npm install react-packery-component --save`

#### Usage:

* The component bundles Packery, so no additional dependencies needed!
* You can optionally include Packery as a script tag if there should be any reason for doing so:
`<script src='//cdnjs.cloudflare.com/ajax/libs/packery/1.3.0/packery.pkgd.min.js' />`

* To use the component just require the module
* Example es5 code:

```js
var React = require('react');
var Packery = require('react-packery-component');

var packeryOptions = {
    transitionDuration: 0
};

var Gallery = React.createClass({
    render: function () {
        var childElements = this.props.elements.map(function(element){
           return (
                <li className="image-element-class">
                    <img src={element.src} />
                </li>
            );
        });

        return (
            <Packery
                className={'my-gallery-class'} // default ''
                elementType={'ul'} // default 'div'
                options={packeryOptions} // default {}
                disableImagesLoaded={false} // default false
            >
                {childElements}
            </Packery>
        );
    }
});

module.exports = Gallery;
```
* Example es2015 code:

```js
import React,{Component} from 'react';
import Packery from 'react-packery-component';

const packeryOptions = {
    transitionDuration: 0
};

class Gallery extends Component {
    constructor() {
        super();
    }
    render() {
        const childElements = this.props.elements.map((element) => {
           return (
                <li className="image-element-class">
                    <img src={element.src} />
                </li>
            );
        });

        return (
            <Packery
                className={'my-gallery-class'} // default ''
                elementType={'ul'} // default 'div'
                options={packeryOptions} // default {}
                disableImagesLoaded={false} // default false
            >
                {childElements}
            </Packery>
        );
    }
});

export default Gallery;
```

### Accessing the Packery instance

Should you need to access the instancy of Packery, you can do so by using the `ref` callback from the component.

```js
import React,{Component} from 'react';
import Packery from 'react-packery-component';

class Gallery extends Component {
    handleLayoutComplete: function() { },

    componentDidMount: function() {
      this.packery.on('layoutComplete', this.handleLayoutComplete);
    },

    componentWillUnmount: function() {
      this.packery.off('layoutComplete', this.handleLayoutComplete);
    }

    render() {
        const childElements = this.props.elements.map((element) => {
           return (
                <li className="image-element-class">
                    <img src={element.src} />
                </li>
            );
        });

        return (
            <Packery
                ref={function(c) {this.packery = this.packery || c.packery;}.bind(this)}
            >
                {childElements}
            </Packery>
        );
    }
});

export default Gallery;
```

### Setting the ImageLoadedEvent 

```js
import React,{Component} from 'react';
import Packery from 'react-packery-component';

const packeryOptions = {
    transitionDuration: 0
};

export default class Gallery extends Component {

    render() {
        const childElements = this.props.elements.map((element) => {
           return (
                <li className="image-element-class">
                    <img src={element.src} />
                </li>
            );
        });

        return (
            <Packery
                className={'my-gallery-class'} // default ''
                elementType={'ul'} // default 'div'
                options={packeryOptions} // default {}
                disableImagesLoaded={false} // default false
                imagesLoadedEvent={[
                    {
                        name: "always",
                        event: (instance)=>console.log('ALWAYS - all images have been loaded')
                    },
                    {
                        name: "done",
                        event: (instance) => console.log("DONE  - all images have been successfully loaded") 
                    },
                    {
                        name: "fail",
                        event: (instance)=> console.log("FAIL - all images loaded, at least one is broken")
                    },
                    {
                        name: "progress",
                        event: (instance, image)=> {
                            let result = image.isLoaded ? 'loaded' : 'broken';
                            console.log( 'image is ' + result + ' for ' + image.img.src );
                        }
                    }
                ]}

            >
                {childElements}
            </Packery>
        );
    }
}

```