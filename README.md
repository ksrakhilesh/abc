# Design an Expense Sharing System

> You are tasked with designing an expense sharing system that allows users to track shared expenses among a group of people. The system should support the following functionalities:

1. **Add Expense:**

   - Create a function that enables a user to add an expense.
   - Parameters:
     - Main user (the one who paid)
     - Amount of the expense
     - List of users included in the payment

   **Examples**

   ```typescript
   expenseSystem.addExpense("A", 1000, ["B", "C", "D"]);
   expenseSystem.addExpense("B", 1200, ["A", "C"]);
   expenseSystem.addExpense("C", 400, ["A", "B", "D"]);
   ```

2. **List User's Expenses:**

   - Implement a function that lists all the expenses a given user is involved in.
   - Details to include: Main user, amount, and the other participants.

   **Example 1: User D**

   ```typescript
   const userAExpenses = expenseSystem.listUserExpenses("D");
   ```

   **Expected Output:**

   ```
   // 1. A Pays 1000 with B, C, and D
   // 2. C Pays 400 with A, B, and D
   ```

   **Example 2: User C**

   ```typescript
   const userAExpenses = expenseSystem.listUserExpenses("C");
   ```

   **Expected Output:**

   ```
   // 1. A Pays 1000 with B, C, and D
   // 2. B Pays 1200 with A and C
   // 3. C Pays 400 with A, B, and D
   ```

3. **Generate Individual Summary:**

   - Develop a function that generates a summary for each individual user.
   - The summary should summarize all the expenses they are involved in and whether they owe money or are owed money.

   **Examples**

   ```typescript
   expenseSystem.addExpense("A", 1000, ["B", "C", "D"]);
   const userASummary = expenseSystem.generateIndividualSummary("A");
   // After the above expense, the following summary is generated for A:
   ```

   ```
    B owes A 250
    C owes A 250
    D owes A 250
   ```

   ```typescript
   expenseSystem.addExpense("B", 1200, ["A", "C"]);
   // After the above expense, the following summary is generated for A:
   const userASummary = expenseSystem.generateIndividualSummary("A");
   ```

   ```
     A owes B 150 ( 400 - 250 )
     C owes A 250
     D owes A 250
   ```

   ```typescript
   expenseSystem.addExpense("C", 400, ["A", "B", "D"]);
   // After the above expense, the following summary is generated for A:
   const userASummary = expenseSystem.generateIndividualSummary("A");
   ```

   ```
   A owes B 150 ( 400 - 250 )
   C owes A 150 ( 250 - 100 )
   D owes A 250
   ```

4. **Settle Funds:**

   - Design a function that calculates and performs fund settlements between two users.
   - Given two users, find the simplest way to settle the debts if any exist.

   **Examples**

   ```typescript
   // Example Function call
   // Just pass 2 users and get the settlement b/w these 2 users
   const settlementsAB = expenseSystem.settleFunds("A", "B");
   ```

   **Example after each expense added:**

   ```
    expenseSystem.addExpense("A", 1000, ["B", "C", "D"]);
    // After the above expense, the following should be the settlements

    > settleFunds("A", "B"):
    Output: B owes A 250

    > settleFunds("A", "C"):
    Output: C owes A 250

    expenseSystem.addExpense("B", 1200, ["A", "C"]);
    // After the above expense, the following should be the settlements

    > settleFunds("A", "B"):
    Output: A owes B 150

    > settleFunds("A", "C"):
    Output: C owes A 250

    > settleFunds("B", "C"):
    Output: C owes B 400


    expenseSystem.addExpense("C", 400, ["B", "D", "F"]);
    // After the above expense, the following should be the settlements

    > settleFunds("A", "B"):
    Output: A owes B 150

    > settleFunds("A", "C"):
    Output: C owes A 150
   ```

   ### Constraints

   - Treat this as a ds-algo problem, and you are expected to implement the solution in a language of your choice.
   - Add Expense can have any time complexity, but the all other functions should have the least time complexity possible.
   - Space complexity should be optimized but not at the cost of time complexity.

---
