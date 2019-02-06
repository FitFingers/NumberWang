# NumberWang
Another tiny practice project to see how data flows in React.

-----

In this small project I wanted to create a number-guessing game and whilst making it, I had trouble with the flow of information.
Unlike vanilla JavaScript, where variables are updated immediately, React uses a unidirectional data flow and also sometimes batches render updates together. This meant that when I updated the state of a parent component through bubbling, I didn't get the results I had expected. For example, when setting the initial guess, I wanted also to set the "highest" state/boundary to be equal to the guess, like so:

    setFirstGuess() {
      this.setState({
        guess: Math.ceil(Math.random() * 1000),
        highest: this.state.guess,
      });
    }

Unfortunately, this doesn't work in React as the state is not updated but rather the two updates are batched together and updated at the same time, leaving "guess" to change correctly but "highest" to remain at the value of the previous state of "guess".

To remedy this, setState() allows a second parameter, usually a callback function, which handles the state changes using the new values:
    
    setFirstGuess() {
      this.setState({
        guess: Math.ceil(Math.random() * 1000),
      }, () => {
        this.setState({
          highest: this.state.guess,
        });
      });
    }
    
This helped a lot, although I have yet to figure out how to pass a function in that has already been declared. As such, any instance of setState(x, y) that I wrote had to use an anonymous arrow function as the second/y parameter.
