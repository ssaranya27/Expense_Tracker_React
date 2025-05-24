# Expense Tracker (ReactJS)
## Date: 24/05/2025

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM

### App.js
```
import React, { useState } from 'react';
import './App.css';

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState('');
  const [amount, setAmount] = useState('');

  const addTransaction = (e) => {
    e.preventDefault();
    if (!description || !amount) return;
    
    const newTransaction = {
      id: Date.now(),
      description,
      amount: parseFloat(amount)
    };
    
    setTransactions([...transactions, newTransaction]);
    setDescription('');
    setAmount('');
  };

  const deleteTransaction = (id) => {
    setTransactions(transactions.filter(transaction => transaction.id !== id));
  };

  const balance = transactions.reduce((acc, transaction) => acc + transaction.amount, 0);
  const income = transactions
    .filter(transaction => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
  const expenses = transactions
    .filter(transaction => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);

  return (
    <div className="app">
      <header className="header">
        <h1>Expense Tracker</h1>
        <div className="balance-box">
          <h3>Your Balance</h3>
          <h2>${balance.toFixed(2)}</h2>
        </div>
        <div className="summary">
          <div className="income">
            <h4>INCOME</h4>
            <p className="money plus">+${income.toFixed(2)}</p>
          </div>
          <div className="expense">
            <h4>EXPENSE</h4>
            <p className="money minus">-${Math.abs(expenses).toFixed(2)}</p>
          </div>
        </div>
      </header>

      <div className="transaction-list">
        <h3>Transactions</h3>
        <ul>
          {transactions.map(transaction => (
            <li key={transaction.id} className={transaction.amount > 0 ? 'plus' : 'minus'}>
              <span>{transaction.description}</span>
              <span>{transaction.amount > 0 ? '+' : ''}{transaction.amount.toFixed(2)}</span>
              <button 
                onClick={() => deleteTransaction(transaction.id)}
                className="delete-btn"
              >
                ×
              </button>
            </li>
          ))}
        </ul>
      </div>

      <form onSubmit={addTransaction} className="transaction-form">
        <h3>Add New Transaction</h3>
        <div className="form-control">
          <label htmlFor="description">Description</label>
          <input
            type="text"
            id="description"
            value={description}
            onChange={(e) => setDescription(e.target.value)}
            placeholder="Enter description..."
          />
        </div>
        <div className="form-control">
          <label htmlFor="amount">Amount</label>
          <input
            type="number"
            id="amount"
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
            placeholder="Enter amount..."
            step="0.01"
          />
          <small>(Positive for income, negative for expense)</small>
        </div>
        <button type="submit" className="btn">Add Transaction</button>
      </form>
    </div>
  );
}

export default App;
```

### App.css
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.app {
  background: white;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 500px;
  padding: 25px;
  margin: 20px 0;
}

.header {
  text-align: center;
  margin-bottom: 20px;
}

.header h1 {
  color: #2c3e50;
  margin-bottom: 20px;
  font-size: 28px;
  text-decoration: underline;
  text-decoration-color: #3498db;
}

.balance-box {
  background: linear-gradient(to right, #3498db, #2c3e50);
  color: white;
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 20px;
}

.balance-box h3 {
  font-size: 16px;
  margin-bottom: 5px;
}

.balance-box h2 {
  font-size: 28px;
}

.summary {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.income, .expense {
  flex: 1;
  text-align: center;
  padding: 15px;
  border-radius: 8px;
}

.income {
  background-color: rgba(46, 204, 113, 0.1);
  margin-right: 10px;
}

.expense {
  background-color: rgba(231, 76, 60, 0.1);
  margin-left: 10px;
}

.money {
  font-weight: bold;
  font-size: 20px;
}

.plus {
  color: #2ecc71;
}

.minus {
  color: #e74c3c;
}

.transaction-list h3 {
  border-bottom: 1px solid #eee;
  padding-bottom: 10px;
  margin-bottom: 15px;
  color: #2c3e50;
}

.transaction-list ul {
  list-style-type: none;
}

.transaction-list li {
  background-color: #f9f9f9;
  padding: 12px;
  margin-bottom: 10px;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
  border-right: 4px solid;
}

.transaction-list li.plus {
  border-color: #2ecc71;
}

.transaction-list li.minus {
  border-color: #e74c3c;
}

.delete-btn {
  background-color: #e74c3c;
  color: white;
  border: none;
  border-radius: 50%;
  width: 25px;
  height: 25px;
  font-size: 16px;
  line-height: 25px;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.3s;
}

.transaction-list li:hover .delete-btn {
  opacity: 1;
}

.transaction-form {
  margin-top: 30px;
}

.transaction-form h3 {
  border-bottom: 1px solid #eee;
  padding-bottom: 10px;
  margin-bottom: 15px;
  color: #2c3e50;
}

.form-control {
  margin-bottom: 15px;
}

.form-control label {
  display: block;
  margin-bottom: 5px;
  color: #2c3e50;
  font-weight: 500;
}

.form-control input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
}

.form-control small {
  color: #7f8c8d;
  font-size: 12px;
}

.btn {
  width: 100%;
  padding: 12px;
  background: linear-gradient(to right, #3498db, #2c3e50);
  color: white;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

@media (max-width: 500px) {
  .app {
    padding: 15px;
  }
  
  .summary {
    flex-direction: column;
  }
  
  .income, .expense {
    margin: 5px 0;
  }
}
```


## OUTPUT
![Screenshot 2025-05-24 111618](https://github.com/user-attachments/assets/3348e4e9-2a8c-46d8-ab61-beac8c34de4e)


## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
