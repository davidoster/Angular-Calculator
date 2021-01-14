# Steps to install Angular #

npm install @angular/cli
ng new example-calculator (example_calculator), y/N/CSS
cd ./example-calculator
ng serve

ng generate component calculator --skipTests

src/app/app.component.html <---
<app-calculator></app-calculator>

src/app/calculator/calculator.component.html <--- (http://jsfiddle.net/ayoisaiah/c8b9zsaq/)
<div class="calculator">

  <input type="text" class="calculator-screen" value="0" disabled />

  <div class="calculator-keys">

    <button type="button" class="operator" value="+">+</button>
    <button type="button" class="operator" value="-">-</button>
    <button type="button" class="operator" value="*">&times;</button>
    <button type="button" class="operator" value="/">&divide;</button>

    <button type="button" value="7">7</button>
    <button type="button" value="8">8</button>
    <button type="button" value="9">9</button>


    <button type="button" value="4">4</button>
    <button type="button" value="5">5</button>
    <button type="button" value="6">6</button>


    <button type="button" value="1">1</button>
    <button type="button" value="2">2</button>
    <button type="button" value="3">3</button>


    <button type="button" value="0">0</button>
    <button type="button" class="decimal" value=".">.</button>
    <button type="button" class="all-clear" value="all-clear">AC</button>

    <button type="button" class="equal-sign" value="=">=</button>

  </div>
</div>

src/app/calculator/calculator.component.css <---
.calculator {
    border: 1px solid #ccc;
    border-radius: 5px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 400px;
  }

  .calculator-screen {
    width: 100%;
    font-size: 5rem;
    height: 80px;
    border: none;
    background-color: #252525;
    color: #fff;
    text-align: right;
    padding-right: 20px;
    padding-left: 10px;
  }

  button {
    height: 60px;
    background-color: #fff;
    border-radius: 3px;
    border: 1px solid #c4c4c4;
    background-color: transparent;
    font-size: 2rem;
    color: #333;
    background-image: linear-gradient(to bottom,transparent,transparent 50%,rgba(0,0,0,.04));
    box-shadow: inset 0 0 0 1px rgba(255,255,255,.05), inset 0 1px 0 0 rgba(255,255,255,.45), inset 0 -1px 0 0 rgba(255,255,255,.15), 0 1px 0 0 rgba(255,255,255,.15);
    text-shadow: 0 1px rgba(255,255,255,.4);
  }

  button:hover {
    background-color: #eaeaea;
  }

  .operator {
    color: #337cac;
  }

  .all-clear {
    background-color: #f0595f;
    border-color: #b0353a;
    color: #fff;
  }

  .all-clear:hover {
    background-color: #f17377;
  }

  .equal-sign {
    background-color: #2e86c0;
    border-color: #337cac;
    color: #fff;
    height: 100%;
    grid-area: 2 / 4 / 6 / 5;
  }

  .equal-sign:hover {
    background-color: #4e9ed4;
  }

  .calculator-keys {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 20px;
    padding: 20px;
  }

src/styles.css
html {
    font-size: 62.5%;
    box-sizing: border-box;
  }

*, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: inherit;
}

src/app/calculator/calculator.component.ts <---
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-calculator',
  templateUrl: './calculator.component.html',
  styleUrls: ['./calculator.component.css']
})

export class CalculatorComponent implements OnInit {
  currentNumber = '0';
  firstOperand: any;
  operator: any;
  waitForSecondNumber = false;

  constructor() { }

  ngOnInit(): void {
  }

  public getNumber(v: string){
    console.log(v);
    if(this.waitForSecondNumber)
    {
      this.currentNumber = v;
      this.waitForSecondNumber = false;
    }else{
      this.currentNumber === '0'? this.currentNumber = v: this.currentNumber += v;

    }
  }

  getDecimal(){
    if(!this.currentNumber.includes('.')){
        this.currentNumber += '.'; 
    }
  }

  private doCalculation(op: any , secondOp: number){
    switch (op){
      case '+':
        return this.firstOperand += secondOp; 
      case '-': 
        return this.firstOperand -= secondOp; 
      case '*': 
        return this.firstOperand *= secondOp; 
      case '/': 
        return this.firstOperand /= secondOp; 
      case '=':
        return secondOp;
    }
    return null;
  }

  public getOperation(op: string){
    console.log(op);

    if(this.firstOperand === null){
      this.firstOperand = Number(this.currentNumber);

    }else if(this.operator){
      const result = this.doCalculation(this.operator , Number(this.currentNumber))
      this.currentNumber = String(result);
      this.firstOperand = result;
    }
    this.operator = op;
    this.waitForSecondNumber = true;

    console.log(this.firstOperand);

  }

  public clear(){
    this.currentNumber = '0';
    this.firstOperand = null;
    this.operator = null;
    this.waitForSecondNumber = false;
  }  
}


src/calculator.component.html
<p>calculator works!</p>

<div class="calculator">

    <input type="text" class="calculator-screen" [value]="currentNumber" disabled />

    <div class="calculator-keys">

        <button type="button" (click)="getOperation('+')" class="operator" value="+">+</button>
        <button type="button" (click)="getOperation('-')" class="operator" value="-">-</button>
        <button type="button" (click)="getOperation('*')" class="operator" value="*">&times;</button>
        <button type="button" (click)="getOperation('/')" class="operator" value="/">&divide;</button>

        <button type="button" (click)="getNumber('7')" value="7">7</button>
        <button type="button" (click)="getNumber('8')" value="8">8</button>
        <button type="button" (click)="getNumber('9')" value="9">9</button>


        <button type="button" (click)="getNumber('4')" value="4">4</button>
        <button type="button" (click)="getNumber('5')" value="5">5</button>
        <button type="button" (click)="getNumber('6')" value="6">6</button>


        <button type="button" (click)="getNumber('1')" value="1">1</button>
        <button type="button" (click)="getNumber('2')" value="2">2</button>
        <button type="button" (click)="getNumber('3')" value="3">3</button>


        <button type="button" (click)="getNumber('0')" value="0">0</button>
        <button type="button" (click)="getDecimal()" class="decimal" value=".">.</button>
        <button type="button" (click)="clear()" class="all-clear" value="all-clear">AC</button>

        <button type="button" (click)="getOperation('=')" class="equal-sign" value="=">=</button>

    </div>
</div>