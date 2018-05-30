# Life cycle

## componentDidMount

component is mounted and ready to be used

load your data

play with DOM, draw on canvas element.

add event listeners

use case: start AJAX calls here

## componentWillReceiveProps

props values are changed and you should react accordingly. do we need to refetch AJAX data?

verify that change has actually happened and act accordingly. Sometimes React just calls this so verify that a change has actually taken place.

## shouldComponentUpdate

should return true. This is about props changing values and React asking for permission to change, given that a change has happened. Default answer is true.

You can improve performance here if you know what you are doing. Example of cases where you wan't to control this, imagine a big table and you only want to change things that matter.

## componentWillUnmount





