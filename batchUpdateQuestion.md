### Q1.) in this below code Why Console.log Show Older Value?

import React from 'react'

// Your task is to explain why the console.log shows the older value of count
function App() {
const [count, setCount] = React.useState(0);

const handleClick = () => {
setCount(count + 1);
console.log(count); // You will see the older value of count in console
};

return (

<div>
<p>Button is clicked {count} times</p>
<button onClick={handleClick}>Click Me</button>
</div>
);
}

export default App

                _Answer:_ In React, when you call setCount(count + 1), it schedules an asynchronous state update rather than applying the change immediately. React adopts this approach for performance optimization. Here's a breakdown of what happens:

                _Asynchronous State Update_: React treats state updates as asynchronous to enhance performance. When setCount is called, React doesn't immediately update the state.

                _Console.log Timing_: When you use console.log(count) immediately after setCount(count + 1), it queries the current value of count before React has completed its state update. The console.log doesn't wait for the asynchronous state update to finish, so it prints the previous value.

                _UI Update_: On the other hand, when you display the count inside the <p> tag, React ensures that the UI updates only after it has completed all scheduled state updates. This means the content inside the <p> tag reflects the latest value because React has finished its asynchronous process and updated the DOM accordingly.

### Q2.) Your task is to explain why count value is not updated to 3 as expected

import React from 'react'

// Your task is to explain why count value is not updated to 3 as expected
function App() {
const [count, setCount] = React.useState(0);

const handleClick = () => {
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);
console.log(count);
};

return (

<div>
<p>Button is clicked {count} times</p>
<button onClick={handleClick}>Click Me</button>
</div>
);
}

export default App

                _Answer:_ When the "Click Me" button is clicked, the handleClick function is executed, and it contains three consecutive calls to setCount(count + 1).

                Now, let's break down what happens during the execution:

                _Synchronous Execution:_ When you call setCount(count + 1), React schedules a state update based on the current state (count at that moment, which is 0).
                Each subsequent setCount is still using the original state value, which is 0, because the updates are asynchronous and haven't been applied yet.

                _Asynchronous State Updates:_ The three setCount calls are queued for state updates, each using the initial state of 0.
                They don't immediately update the state; instead, they are scheduled for later processing.

                _Console Log:_ The console.log(count) statement is executed immediately after the three setCount calls. However, at this point, the state updates have not been applied yet.
                The output of console.log(count) is still the original state, which is 0.

                _Reconciliation Phase:_ After the current synchronous execution completes (including the console.log), React proceeds to the reconciliation phase.
                During reconciliation, React applies the queued state updates one by one.

                _State Update Calculation:_ Each setCount is processed using the initial state of 0, resulting in 0 + 1 for all three updates.
                Therefore, the final state becomes 1, not 3.

**To update the state by 3, you need to ensure that each setCount call uses the latest state value. This can be achieved by using the functional form of setCount, which takes the previous state as an argument:**

                    const handleClick = () => {
                    setCount((prevCount) => prevCount + 1);
                    setCount((prevCount) => prevCount + 1);
                    setCount((prevCount) => prevCount + 1);
                    console.log(count);
                    };
