### 1. key
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




 ### 2. always pass function in ```onclick```

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




### 3.in style tag we need to pass a object always


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


### 4. 
