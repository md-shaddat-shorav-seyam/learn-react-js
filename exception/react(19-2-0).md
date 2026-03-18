# 1. key
you have to add key to list otherwise might show error
```javascript
export function ListObj(p){
    //    const fruits=[{id:1, name:"apple",calories :95},
    //     {id:2, name:"orange",calories :45},
    //     {id:3, name:"banana",calories :105},
    //     {id:4, name:"coconut",calories :159},
    //     {id:5, name:"pineapple",calories :37},

    //    ]
     //fruits.sort((a,b)=>a.calories-b.calories)
    //  fruits.sort((a,b)=>a.name.localeCompare(b.name))




        const fruits = p.items
//    const lowCal = fruits.filter((f)=>f.calories>100)
      


  let fruit = fruits.map((f)=>{return(
    <li key={f.id}>{f.name} have {f.calories} calories </li>
  )})
  return (<>
     <h2 className="font-medium text-red-700">{p.catagory}</h2>
  <ol>{fruit}</ol></>)
}
 ```
 ---




 # 2. always pass function in ```onclick```

 in parameter function if you do not pass function then it runs by its own

 ```javascript
 
export function OnClick() {
  let c = 0;
  const handleClick = () => console.log("i am clicked");
  const handleClick2 = (name) => console.log(` ${name} stop click me`);
  const handleClick3 = (name) => {
    if (c < 4) {
      c++;
      console.log(`${name} ! you  clicked me ${c} times`);
    } else {
      console.log("stop clicking me");
    }
  };

  return <button onClick={() => handleClick3("bro")}> click me</button>;
}


}

 ```
---




# 3. in style tag we need to pass a object always


```js 

import { useState } from "react"

export function ColorPicker(){

    const[color,setColor] = useState("#ffffff")
    return(<div className="flex flex-col bg-gray-700 h-screen w-screen justify-center items-center"> 
                <h1 className="text-5xl mb-3 text-white ">color picker</h1>
    
                <div className={`h-50 w-50 rounded-3xl mt-7 flex flex-col justify-center items-center`} style={{backgroundColor : color}}>
                     <p>selected color :</p>
                     
                      <p  > {color} </p> 
                </div>

                <p className="mt-9 text-shadow-amber-50 bold " style={{ color: color}}> select  color : </p>
                <input onChange={(e)=>setColor(e.target.value)} type="color" value={color}></input>
    
    
    </div>)
}

```


# 4. for using ``` useState()``` for more than one time we need to use updater function


``` 
useState(e=>e+1 )

```
   as in react value are update after batch update of dom but passing a function would transfer them to a queue value update session 

#### for example:

``` js
import { useState } from "react";

export function UpdaterFunc() {
  const [count, setcount] = useState(1);

  const dec = (e) => {
    setcount(count - 1);
  };

  const rst = (e) => {
    setcount(0);
  };

  if (count > 100) {
    setcount(0);
  }

  const inc = (e) => {
    // setcount(count + 1)  we can not do this for two +++ time it donot update second time
    setcount((x) => x + 1);
    setcount((x) => x + 1);
  };

  return (
    <>
      <div className="h-screen w-screen bg-gray-600 flex flex-col items-center justify-center ">
        <p className="text-8xl">{count}</p>
        <div className="flex gap-8 text-3xl mt-9">
          <button
            className="bg-yellow-800 rounded-4xl p-5 hover:bg-yellow-500  "
            onClick={dec}
          >
            decrement
          </button>
          <button
            className="bg-yellow-800 rounded-4xl p-5 hover:bg-yellow-500  "
            onClick={rst}
          >
            reset
          </button>
          <button
            className="bg-yellow-800 rounded-4xl p-5 hover:bg-yellow-500  "
            onClick={inc}
          >
            increment
          </button>
        </div>
      </div>
    </>
  );
}


```


----


# 5. for updateing ubject

#### we need to pass a object with over writing



``` js
import { useState } from "react"

export function UpdateObj(){

     const [objj, sobjj] = useState({ year: 2024, brand: "ford", model: "mustang" })

        return(
            <div>
                <p>your fav car: {objj.year} {objj.brand} {objj.model}</p>
                <p>hi</p>
                <input
                    type="number"
                    value={objj.year}
                    onChange={(e) => sobjj( x=>( { ...x, year: Number(e.target.value) }))}
                />
                <input type="text"
                    value={objj.brand}
                    onChange={e=>sobjj(  x=>( {...x, brand : e.target.value }))}
                />
                <input type="text"
                    value={objj.model}
                    onChange={e=>sobjj(  x=>(  {...x, model : e.target.value }))}
                />
            </div>
        )
}

```



end
---
# 6. we should not get value by query selector














# gpt code++++++++++++++++++++++++++++++++++++++++++++++++
                         You're very close 👍 — but there are **two important React mistakes** in your code:

---

## ❌ Problem 1: Using `document.querySelector` in React

```js
const fList = document.querySelector("#fList")
```

👉 This is **not how React works**. React uses **state + refs**, not direct DOM access.

* This runs **before render**, so `fList` is often `null`
* Breaks React’s declarative model

---

## ❌ Problem 2: Reading input value incorrectly

```js
setList(el => ([...el, fList.value]))
```

Since `fList` is unreliable → this can fail or behave inconsistently.

---

# ✅ Correct Way (React Style)

Use **useState for input** instead of DOM query.

---

## ✅ Fixed Code

```jsx
import { useState } from "react";

export function UpdateArray() {
  const [list, setList] = useState(["apple", "orange", "banana"]);
  const [input, setInput] = useState("");

  return (
    <div className="bg-gray-900 h-screen w-screen flex flex-col justify-center items-center text-[#ffffff]">
      
      <div>
        <h1>List of food</h1>
        <ol>
          {list.map((item, i) => (
            <li
              key={i}
              onClick={() => {
                setList(prev => prev.filter((_, ind) => ind !== i));
              }}
            >
              {i + 1}. {item}
            </li>
          ))}
        </ol>
      </div>

      <div>
        <input
          type="text"
          className="bg-amber-100 text-black"
          placeholder="Enter food name"
          value={input}
          onChange={(e) => setInput(e.target.value)}
        />

        <button
          onClick={() => {
            if (input.trim() === "") return;
            setList(prev => [...prev, input]);
            setInput(""); // clear input
          }}
          className="m-5 bg-amber-900 p-2 rounded-4xl hover:bg-amber-200"
        >
          Add Food
        </button>
      </div>

    </div>
  );
}
```

---

# 🔥 Key Concepts You Just Learned

### ✅ 1. Controlled Components

```js
value={input}
onChange={(e) => setInput(e.target.value)}
```

👉 React controls the input, not the DOM.

---

### ✅ 2. Never use `querySelector` in React (99% cases)

Use:

* `useState` → for values
* `useRef` → if you really need DOM

---

### ✅ 3. Functional state update (good job 👏)

```js
setList(prev => [...prev, input])
```

You already used this correctly 👍

---

# 💡 Small Improvements

* Use better variable names (`item` instead of `e`)
* Avoid using index as key in real apps (okay for learning)

---

If you want, I can show you:

* 🚀 How to use `useRef` version (no state for input)
* 🚀 Add edit/update instead of delete
* 🚀 Persist list in localStorage

Just tell me 👍





++++++++++++++++++++++++++++++++++++++++++++++++++
end
---

# 7. Non mutable state




### here we can not modify ```el``` 
```js 

setArr((el)=>{
  //we cannot do these things
  el.push()
  el.pop() 
  // and others updaters............
})
```



```js

 <FaAngleUp
                  onClick={() => {
                    console.log("clicked up", i);
                    if (i > 0) {
                    setArr((el) => {
                      let elup = [...el]; // el is non mutable & non reference 
                    [elup[i], elup[i-1]] = [elup[i-1], elup[i]];
                   // [elup[i], elup[i-1]] = sww( elup[i-1], elup[i]); // 
                      console.log( el,elup);//if you see this 
                      return elup;

                    }); 
                }
                  }}
                />
```






```js

import { useState } from "react";
import { FaAngleUp } from "react-icons/fa";
import { MdDelete } from "react-icons/md";
import { FaAngleDown } from "react-icons/fa";
export function Mytodo() {
  let sww = (e1, e2) => {console.log("i am printing twice") ;return([e2, e1])}; // that is why function is wappping twice
  const [text, setText] = useState("");
  const [arr, setArr] = useState(["i am todo", "ia"]);
  return (
    <>
      <div className="h-screen w-screen bg-gray-950 text-white  flex flex-col justify-center items-center ">
        <div>
          <input
            type="text"
            placeholder="enter todos"
            onChange={(e) => setText(e.target.value)}
            className="border-2 rounded-4xl p-5"
          />
          <button
            onClick={() => {
              setArr((e) => [...e, text]);
            }}
            className="ml-7 border-2 p-3 rounded-2xl hover:bg-amber-200 hover:text-black"
          >
            Add
          </button>
        </div>

        <ul className="m-7 ">
          {arr.map((e, i) => (
            <li
              className="border-2 w-200 h-20 p-6 flex flex-row justify-between m-2"
              key={i}
            >
              <p> {e} </p>{" "}
              <span className="flex text-4xl gap-4">
                <FaAngleUp
                  onClick={() => {
                    console.log("clicked up", i);
                    if (i > 0) {
                    setArr((el) => {
                      let elup = [...el]; // el is non mutable & non reference 
                    [elup[i], elup[i-1]] = [elup[i-1], elup[i]];
                   // [elup[i], elup[i-1]] = sww( elup[i-1], elup[i]); // 
                      console.log( el,elup);//if you see this 
                      return elup;

                    }); 
                }
                  }}
                />
                <MdDelete
                  onClick={() => {
                    setArr((a) => a.filter((_, ind) => ind !== i));
                  }}
                  color="red"
                />
                <FaAngleDown />{" "}
              </span>{" "}
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}



```



end++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
---



# 8. under state , custom functions run twice
so we should avoid


## eventhough under the state functions run twice but native ``` value ```of  ```const [value, setvalue] = useState()``` and other  updates for onetime 
  
  ```js 
import { useState } from "react";

let d = (e) => {
  e += 1;
  console.log("i am");// this run twice
  return e;
};

export function Dump() {
  const [value, setValue] = useState(0);
  return (
    <>
      <div className="h-screen w-screen flex flex-col justify-center items-center">
        <div>{value}</div>
        <button
          onClick={() => {
            setValue((e) => {
              let t = e;
              t = d(t);
              return t;
            });
          }}
        >
          update
        </button>
      </div>
    </>
  );
}

  ```
## ☝️ here value update once but ``` closole.log()``` print twice

## on the hand  👇below , sww happens twice that's why there is no meaning of swapping




---

```js
import { useState } from "react";
import { FaAngleUp } from "react-icons/fa";
import { MdDelete } from "react-icons/md";
import { FaAngleDown } from "react-icons/fa";
export function Mytodo() {
  let sww = (e1, e2) => {console.log("i am printing twice") ;return([e2, e1])}; // that is why function is wappping twice
  const [text, setText] = useState("");
  const [arr, setArr] = useState(["i am todo", "ia"]);
  return (
    <>
      <div className="h-screen w-screen bg-gray-950 text-white  flex flex-col justify-center items-center ">
        <div>
          <input
            type="text"
            placeholder="enter todos"
            onChange={(e) => setText(e.target.value)}
            className="border-2 rounded-4xl p-5"
          />
          <button
            onClick={() => {
              setArr((e) => [...e, text]);
            }}
            className="ml-7 border-2 p-3 rounded-2xl hover:bg-amber-200 hover:text-black"
          >
            Add
          </button>
        </div>

        <ul className="m-7 ">
          {arr.map((e, i) => (
            <li
              className="border-2 w-200 h-20 p-6 flex flex-row justify-between m-2"
              key={i}
            >
              <p> {e} </p>{" "}
              <span className="flex text-4xl gap-4">
                <FaAngleUp
                  onClick={() => {
                    console.log("clicked up", i);
                    if (i > 0) {
                    setArr((el) => {
                      let elup = [...el]; // el is non mutable & non reference 
                    //[elup[i], elup[i-1]] = [elup[i-1], elup[i]];
                   [elup[i], elup[i-1]] = sww( elup[i-1], elup[i]); // sww runs twice
                      console.log( el,elup);//if you see this run twice
                      return elup;

                    }); 
                }
                  }}
                />
                <MdDelete
                  onClick={() => {
                    setArr((a) => a.filter((_, ind) => ind !== i));
                  }}
                  color="red"
                />
                <FaAngleDown />{" "}
              </span>{" "}
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}

```
