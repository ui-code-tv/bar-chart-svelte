<script>
//Expenses for the month
const data = [
  { name: "Phone", expense: 151 },
  { name: "Electricity", expense: 100 },
  { name: "Car", expense: 5 },
  { name: "House", expense: 43 },
  { name: "Food", expense: 56 },
  { name: "Leisure", expense: 182 }
];
let expensesData = data;
const maxExpense = 200;
const chartHeight = maxExpense + 20;
const barWidth = 50;
const barMargin = 30;
const numberofBars = expensesData.length;
let width = numberofBars * (barWidth + barMargin);

  // Calculate highest expense for the month
  const calculateHighestExpense = (data) => data.reduce((acc, cur) => {
    const { expense } = cur;
    return expense > acc ? expense : acc;    
  }, 0);
  let highestExpense =  calculateHighestExpense(expensesData);

  const createRandomData = (data) => data.map((exp) => ({
    name: exp.name,
    expense: Math.floor(Math.random() * maxExpense)
  }))

  let refreshChart = ()=> {
    expensesData = createRandomData(expensesData);
    highestExpense = calculateHighestExpense(expensesData);
    
  }

</script>

<div id='root'>

  <p class="legend">
    <span class="expense">Expense</span>
    <span class="highest-expense">Highest expense</span>
  </p>

  <svg
  viewBox={`0 0 ${width} ${chartHeight}`}   
  width="100%"
  height="70%"
  preserveAspectRatio="xMidYMax meet"
>{#each expensesData as data,index}
  <rect x={index * (barWidth + barMargin)} y={chartHeight - data.expense} width={barWidth} height={data.expense} fill={highestExpense===data.expense ? `purple` : `black`} /> 
  <text x={index * (barWidth + barMargin) } y={chartHeight - data.expense - 5}>
    {highestExpense === data.expense ? `${data.name}: ${data.expense}` : `${data.expense}`}
  </text>
  {/each}
</svg>
<button on:click={refreshChart}>Refresh Chart</button>

</div>

<style>
:global(html){
  height:100%;
  padding:0;
  margin:0;
  font-family: 'Open Sans', sans-serif; 
  font-size: 2vh;;
}

*{
  box-sizing:border-box;
}

:global(body) {
  
  width:100%;
  height:100%;
  background-color:#ccc;  
  display:flex;
  justify-content:center;
  align-items:center;
  flex-direction: column;
  
}

button {
  display:block;
  width:100%;
  font-size: 4vh;
  
}

svg {

  background-color:orange;
}

#root {
  width:50%;
}

.legend {font-size:2.5vh;}
.expense:before{
	content:'.';
	background-color:black;
	color:black;
	width:3vh;
	display:inline-block;
	margin: 0 0.5vh 0 0.5vh
}
.highest-expense:before{
	content:'.';
	background-color:purple;
	color:purple;
	width:3vh;
	display:inline-block;
  margin: 0 0.5vh 0 2.5vh
}

</style>