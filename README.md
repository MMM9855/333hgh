import React, { Component } from "react";
import o from "./images/orange.svg";
import m from "./images/monkey.svg";
import b from "./images/banana.svg";
import s from "./images/strawberry.svg";
import "./App.scss";

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      wheels: [
        {
          id: 1,
          symbols: [
            { id: 1, symbol: s, name: "S" },
            { id: 2, symbol: b, name: "B" },
            { id: 3, symbol: o, name: "o" },
            { id: 4, symbol: m, name: "m" }
          ]
        },
        {
          id: 2,
          symbols:  [
            { id: 1, symbol: s, name: "S" },
            { id: 2, symbol: b, name: "B" },
            { id: 3, symbol: o, name: "o" },
            { id: 4, symbol: m, name: "m" }
          ]
        },
        {
          id: 3,
          symbols:  [
            { id: 1, symbol: s, name: "S" },
            { id: 2, symbol: b, name: "B" },
            { id: 3, symbol: o, name: "o" },
            { id: 4, symbol: m, name: "m" }
          ]
        },
        {
          id: 4,
          symbols:  [
            { id: 1, symbol: s, name: "S" },
            { id: 2, symbol: b, name: "B" },
            { id: 3, symbol: o, name: "o" },
            { id: 4, symbol: m, name: "m" }
          ]
        }
      ],
      positions: [0, 1, 2, 3],
      rotationDegree: 0,
      isRotating: false,
      result: {},
      hasClickedStartButton: false
    };
  }

  //generate random number between 0 and 4, 4 not inclusive
  generateRandomNumber = () => {
    return Math.floor(Math.random() * 4);
  };

  startRotation = () => {
    if (this.state.isRotating) {
      return;
    }
    this.setState({ isRotating: true, result: {}, hasClickedStartButton: true }, () => {
      this.rotateTimer = setInterval(() => this.rotateWheel(), 50);
      this.rotateTimeout = setTimeout(() => {
        if (this.state.isRotating) {
          this.stopRotation();
        } else {
          clearTimeout(this.rotateTimeout);
        }
      }, 10000);
    });
  };

  //rotate wheel and generate random symbol
  rotateWheel = () => {
    const positions = [...Array(4).keys()];
    positions.forEach((el, i) => {
      positions[i] = this.generateRandomNumber();
    });
    this.setState({ positions });
    this.setState({ rotationDegree: this.state.rotationDegree + 10 });
  };

  //stop rotation
  stopRotation = () => {
    this.clearTimer();
  };

  //clear timeout and interval
  clearTimer = () => {
    clearInterval(this.rotateTimer);
    clearTimeout(this.rotateTimeout);
    this.setState({ isRotating: false }, () => {
      this.decideResult();
    });
  };

  decideResult = () => {
    const positions = [...this.state.positions];
    //check unique count, if count is 0, game is lost, else game is won
    const uniqueCount = positions.length - new Set(positions).size;
    if (uniqueCount > 0) {
      let price = "$10";
      // all are the same, price money is $100
      if (uniqueCount + 1 === positions.length) {
        price = "$100";
      } else {
        //find consecutive, if found, price money is $10
        positions.forEach((el, i) => {
          if (positions[i] === positions[i - 1] || positions[0] === positions[positions.length - 1]) {
            price = "$20";
          }
        });
      }
      this.setState({ result: { type: 1, text: "Winner", price: price } }, () => {});
    } else {
      this.setState({ result: { type: 2, text: "Lost" } });
    }

    this.setState({ positions }, () => {
      setTimeout(() => {
        this.setState({ result: {} });
      }, 4000);
    });
  };

  componentDidMount() {
    //start rotation is user doesn't do anything in 5 seconds
    setTimeout(() => {
      if (!this.state.hasClickedStartButton) {
        this.startRotation();
      }
    }, 5000);
  }

  componentWillUnmount() {
    clearInterval(this.rotateTimer);
    clearTimeout(this.rotateTimeout);
  }
  $(document).ready(function {$(button ).click(function{$(.p).css{display:show});});});



->render 
  render() {
    const { rotationDegree, positions, isRotating, result } = this.state;
    return (
      <head> Paktolus solutions<head>
      <div className="pic">
      <p className="symbol" style="display:None">♠, ♥, ♦, ♣ </p>
      <div>
      <div className="app"> 
        <div className="wheels">
          <div className="each-wheel" style={{ transform: `rotate(${rotationDegree}deg)` }}>
            {this.state.wheels.map((wheel, i) => (
             
            ))}
          </div>
        </div>

        <div className="actions">
          <button onClick={() => this.stopRotation()} disabled={!isRotating}>
            Stop
          </button>
          <button onClick={() => this.startRotation()} disabled={isRotating}>
            Start
          </button>
<button className="button">
 start the game
</button>
        </div>
        {result.type && (
          <div className="toast show" style={{ backgroundColor: result.type === 1 ? "#4BB543" : "#bb2124" }}>
            {result.type === 1 ? <div>You just won {result.price}</div> : <div>You lost, try again later</div>}
          </div>
        )}
      </div>
    );
  }
}

export default App;
->scss

.app {
  font-size: 14px;
  max-width: 700px;
  margin: 50px auto;
 
  padding: 15px;
  border-radius: 5px;
  @media (max-width: 767px) {
    box-shadow: none;
    margin: 10px auto;
  }
  .wheels {
    width: 280px;
    height: 280px;
    border-radius: 50%;
    position: relative;
    overflow: hidden;
    border: 17px solid red;
  
    transform: rotate(0deg);
    background: green;
    margin: 0 auto;
    &:before {
      position: absolute;
      border: 4px pink;
      width: 245px;
      height: 245px;
      border-radius: 50%;
      z-index: 1000;
      left: 1px;
      left: 1px;
    }
    .each-wheel {
      width: 100%;
      height: 100%;
     
      &-section {
        position: absolute;
        width: 0;
        height: 0;
        border-style: solid;
        border-width: 140px 140px 0;
      
        transform-origin: 140px 140px;
        left: 0px;
        top: 0px;
        opacity: 1;
        background: transparent;

        img {
          height: 45px;
          width: 45px;
        }
        &:nth-child(1) {
          transform: rotate(45deg);
          border-color: light blue transparent;
        }
        &:nth-child(2) {
          transform: rotate(135deg);
          border-color: light pink transparent;
        }
        &:nth-child(3) {
          transform: rotate(225deg);
          border-color: light yellow transparent;
        }
        &:nth-child(4) {
          transform: rotate(315deg);
          border-color: light green transparent;
        }
        span {
          margin-top: -115px;
          color: rgba(55, 51, 42, 0.2);
          position: relative;
          z-index: 4;
          display: block;
          text-align: center;
          font-size: 36px;
          margin-left: -20px;

         
        }
      }
    }
  }
  .results {
    margin: 30px 0px;
  }
  .actions {
    margin: 20px -20px;
    display: flex;
    justify-content: center;
    button {
      margin: 20px;
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      outline: 0;
      color: white;
      border: 0;
      padding: 10px 15px;
      background-color: blue;
      border-radius: 3px;
      width: 150px;
      cursor: pointer;
      font-size: 18px;
      transition-duration: 0.3s;
      &:disabled {
        opacity: 0.2;
        background: grey;
        pointer-events: none;
      }
    }
  }
  .toast {
    visibility: hidden;
    min-width: 250px;
    margin-left: -125px;
    color: #fff;
    text-align: center;
    border-radius: 20px;
    padding: 16px;
    position: fixed;
    z-index: 1;
    left: 50%;
    bottom: 20px;
    font-size: 17px;

    &.show {
      visibility: visible;
     
    }

    @-webkit-keyframes fadein {
      from {
        bottom: 0;
        opacity: 0;
      }
      to {
        bottom: 20px;
        opacity: 1;
      }
    }
 fadein {
      from {
        bottom: 0;
        opacity: 0;
      }
      to {
        bottom: 20px;
        opacity: 1;
      }
    }

     fadeout {
      from {
        bottom: 20px;
        opacity: 1;
      }
      to {
        bottom: 0;
        opacity: 0;
      }
    fadeout {
      from {
        bottom: 20px;
        opacity: 1;
      }
      to {
        bottom: 0;
        opacity: 0;
      }
    }
  }
}


import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
->

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(<App />, document.getElementById('root'));

serviceWorker.unregister();
