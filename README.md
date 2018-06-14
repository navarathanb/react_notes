# react_notes


Notes : 
javascript Map() function is using to display array of numbers.

const arr = [1,2,3,4];
const dbl = arr.map((arr)=>
  <li>{arr * 2}</li>
)

and render into ul.
ReactDom.render(
  <ul>{dbl}</ul>, document.getElementById("tag");
);

KYES: 
are using to identify the array items in which to changing , adding or deleting the items, it is assigned to element to identify in the array.

function NumberList(props){
  const values = props.numbers;
  const listItems = values.map((numbe,i) => 
      <li key={i}>
         { numbe }
      </li>
   );
return (
<ul>{listItems}</ul>                         
);                          
}
                       

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(<NumberList  numbers={numbers} />, 
                document.getElementById('root')
);
