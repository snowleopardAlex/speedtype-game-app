// React Hooks don't work well with setInterval, better use setTimeOut
import {useEffect, useState, useRef } from "react";
import './App.css';

function App() {
  const STARTING_TIME = 5

  const [text, setText] = useState("")
  const [timeRemaining, setTimeRemaining] = useState(STARTING_TIME)
  const [isTimeRunning, setIsTimeRunning] = useState(false)
  const [wordCount, setWordCount] = useState(0)
  const textBoxRef = useRef(null)
  
  function handleChange(e) {
    const {value} = e.target
    setText(value)
  }

  // function that calculates words
  // text refers only to this function, it will not modify anything outside of it
  function calculateWordCount(text) {
    const wordsArr = text.trim().split(" ")
    return wordsArr.filter(word => word !== "").length
  }

  function startClock() {
    setIsTimeRunning(true)
    setTimeRemaining(STARTING_TIME)
    setText("")
    textBoxRef.current.disabled = false
    textBoxRef.current.focus()
  }

  function endGame() {
    setIsTimeRunning(false)
    setWordCount(calculateWordCount(text))
  }

  // 
  useEffect(() => {
    // when stops at 0, it counts no more. 
    if(isTimeRunning && timeRemaining > 0) {
      setTimeout(() => {
        setTimeRemaining(time => time - 1)
      }, 1000);
    } else if(timeRemaining === 0) {
        endGame()
    }
  }, [timeRemaining, isTimeRunning])


  return (
    // {() => calculateWordCount(text)} -> anonymous function
    // it was declared without any named identifier to refer to it. 
    <div className="App">
      <h1>How fast do you type?</h1>
      <textarea
        ref={textBoxRef} 
        onChange={handleChange}
        value={text}
        disabled={!isTimeRunning}
      />
      <h4>Time remaining: {timeRemaining}</h4>
      <button 
        disabled={isTimeRunning}
        onClick={startClock}>Start</button>
      <h1>Word count: {wordCount}</h1>
    </div>
  );
}

export default App;