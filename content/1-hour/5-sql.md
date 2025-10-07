# Working with SQL Server Using GitHub Copilot

GitHub Copilot for the MSSQL extension brings AI-powered assistance directly into your SQL development workflow within Visual Studio Code. This integration enables developers to work more efficiently with SQL Server, Azure SQL, and Microsoft Fabric databases by:

- **Writing and optimizing queries** - Generate SQL queries from natural language descriptions and receive AI-recommended improvements for performance
- **Exploring and designing schemas** - Understand, design, and evolve database schemas using intelligent, code-first guidance with contextual suggestions for relationships and constraints
- **Understanding existing code** - Get natural language explanations of stored procedures, views, and functions to help you understand business logic quickly
- **Generating test data** - Create realistic, schema-aware sample data to support testing and development environments
- **Analyzing security** - Receive recommendations to avoid SQL injection, excessive permissions, and other security vulnerabilities
- **Accelerating development** - Scaffold backend components and data access layers based on your database context

This powerful combination allows you to focus on solving problems rather than memorizing SQL syntax, making database development more intuitive and productive.

## Scenario

Now that your application supports multiple database systems including SQL Server, you want to explore how GitHub Copilot can help you interact with your SQL Server database directly from VS Code. You'll use the GitHub Copilot for MSSQL extension to generate queries and explore your data using natural language.

## Prerequisites

Good news! We've already set up everything you need for this exercise:

- SQL Server Express is installed and running locally 
  - With pets database populated with data
- The **GitHub Copilot for MSSQL** extension is installed in VS Code
- A SQL Server connection has been pre-registered in your environment

You're ready to connect and start querying right away!

## Understanding GitHub Copilot for MSSQL

The GitHub Copilot for MSSQL extension brings AI-powered assistance to your database work. It can:

- Generate SQL queries from natural language descriptions
- Explain existing queries in plain English
- Suggest query optimizations and best practices
- Help explore database schemas and relationships
- Convert natural language questions into executable SQL

This integration makes working with databases more intuitive, especially when you're exploring unfamiliar schemas or need to write complex queries quickly.

## Connect to SQL Server

First, let's establish a connection to your SQL Server instance.

1. []  Open the **SQL Server** view by clicking on the SQL Server icon in the Activity Bar (left sidebar).
  - ![MSSQL server extension](images/5-mssql-extension.png)
2. []  For your convenience we have already registered **LocalServer** for you.
3. []  Expand **LocalServer** and you should see **Databases**, **Security** and **Server Objects** nodes as a tree. Expand the **Databases** node to see the **PetsDB** database.

## Optional: Create database

> [!NOTE]
> We've already provided a pre-seeded **PetsDB** database for you. This section is optional if you want to practice creating a database from scratch using Copilot's inline mode. 

If you want to create your own database:

1. []  In the SQL Server view, right-click on your server connection and select **New Query**.
2. []  Press <kbd>Control</kbd>+<kbd>I</kbd> to open Copilot inline chat in the query editor.
3. []  Type the following prompt:

    ```text
    Create a database called Pets with recovery mode set to simple
    ```

4. []  Review the generated SQL and click **Accept** the suggestion if it feels right. It should be very similar to this:

    ```sql-nocopy
    CREATE DATABASE Pets;
    ALTER DATABASE Pets SET RECOVERY SIMPLE;
    ```
    
5. []  Execute the query by clicking **Run** or pressing <kbd>Control</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd>.
6. []  Refresh the SQL Server view to see your new **Pets** database.

Typically we would now seed the just created database by calling the **seed-database.ps1**, but let's skip so we are not distracted by non Copilot activities.

## Query the database using natural language

Now let's use GitHub Copilot to query our database using natural language. This is where the power of AI-assisted SQL really shines!

1. []  In the SQL Server view, expand your connection and the **PetsDB** database to see the tables.
2. []  Right-click on the **PetsDB** database and select **New Query** to open a query editor.
  - This will force the query editor to be included in Copilot context
3. []  Open GitHub Copilot Chat if it's not already open and start a new chat by clicking the **+** button and make sure you are in **Agent** mode.
4. []  If available, select **GPT-4.1** from the list of available models.
5. []  In Copilot Chat, type the following natural language question:

    ```text
    @mssql How many "Golden Retrievers" do we have available for adoption in the Database?
    ```

By using the **@mssql** chat participant we ensure that Copilot will optimally use SQL Server capabilities.


> [!TIP]
> Notice how Copilot understands the context of your database and generates a SQL query that:
> - Joins the necessary tables (dogs and breeds)
> - Filters by breed name
> - Filters by availability status
> - Counts the results

7. []  Review the generated SQL query. It should look something like:

    ```sql-nocopy
    SELECT COUNT(*) as available_golden_retrievers
    FROM dogs d
    JOIN breeds b ON d.breed_id = b.id
    WHERE b.name = 'Golden Retriever'
    AND d.available = 'Available';

    ```

8. Execute the query by issuing the prompt `@mssql run the query` or `@mssql /runQuery` and Copilot will show the results on the chat window.

8. If you want, you can hover the returned query and click on **Insert at Cursor** icon (the second icon from the right, or use the copy and then paste the query).
9. Execute the query by clicking on Green right arrow or press <kbd>Control</kbd>+<kbd>Shift</kbd>+<kbd>E</kbd>.
10. []  View the results in the Results pane below the query editor.

> [!IMPORTANT]
> Because LLMs are probabilistic, not deterministic, the exact SQL generated can vary. The query should accomplish the same goal even if the syntax differs slightly.


## Exercise: Explore more queries (optional)

If you want to get some ideas about some more complex queries that Copilot can generate, try asking Copilot these questions:

> [!TIP]
> You can execute the same prompts directly in the query window by using inline chat with <kbd>Control</kbd>+<kbd>I</kbd> accept and then execute it.


1. []  **Find all available dogs with their breed names**: Ask Copilot to generate a query that shows dog names, ages, and breed names for all dogs that are available for adoption.

    > [!Hint]
    > Try a prompt like: "Show me all available dogs with their names, ages, and breed names"

2. []  **Group dogs by breed**: Ask Copilot to count how many dogs of each breed are in the database.

    > [!Hint]
    > Try: "Count the number of dogs for each breed and show the breed name"

3. []  **Find the oldest dog**: Ask Copilot to find the name and age of the oldest dog in the shelter.

    > [!Hint]
    > Try: "Find the oldest dog in the database"

4. []  **Complex filtering**: Ask Copilot to find all available dogs that are younger than 5 years old, grouped by breed.

    > [!Hint]
    > Try: "Show me available dogs younger than 5 years old, grouped by breed with counts"

For each query:

1. []  Ask Copilot your question in natural language.
2. []  Review the generated SQL.
3. []  Copy it to the query editor and execute it.
4. []  Verify the results make sense.

## Understanding query explanations

Copilot can also help you understand existing SQL queries. This is incredibly useful when working with complex queries or legacy code.

1. []  Copy a complex query from your previous exercises into the query editor.
2. []  Select the entire query.
3. []  In Copilot inline Chat, type **/explain**
4. []  Read through Copilot's explanation of what the query does, including details about joins, filters, and aggregations.

> [!TIP]
> If the text is too big and hard to read because of the scrolling click on **View Chat**

> [!NOTE]
> This feature is particularly helpful when inheriting code from other developers or when returning to queries you wrote months ago.

## Exercise: Generate test data with the @mssql chat participant

GitHub Copilot can help you generate realistic test and mock data for your database. This is especially useful when you need to test your application with meaningful data, create demos, or simulate edge cases. Let's use the **@mssql** chat participant to generate test data.

> [!NOTE]
> The **@mssql** chat participant is specifically designed for SQL Server operations and requires an active database connection to understand your schema context.

### Generate edge case test data

Testing edge cases is crucial for building robust applications. Let's generate data to test boundary conditions or uncover other issues our code might have with odd data shapes.

1. []  Ask Copilot to generate edge case data:

    ```text
    @mssql Generate insert statements for the dogs table to test edge cases. Include:
    - A dog with age 0 (newborn puppy)
    - A dog with age 20 (very old)
    - Dogs with very short names (1-2 characters)
    - Dogs with longer names (15+ characters)
    - Other edge case variations you can think of
    All should reference valid breed IDs.
    ```

2. []  Review the generated SQL to see how Copilot handles these edge cases.
3. []  Execute the statements.

> [!IMPORTANT]
> Edge case testing helps you discover potential issues before they affect real users. Pay attention to how your UI displays very long names, ages at boundaries, etc.

## Summary and next steps

You've successfully used GitHub Copilot with the MSSQL extension to interact with SQL Server using natural language! You learned how to:

- Connect to SQL Server from VS Code
- Generate SQL queries from natural language questions
- Execute and validate query results
- Use Copilot to explore and understand database queries

This integration dramatically reduces the friction of working with databases, allowing you to focus on the questions you want to answer rather than the syntax required to answer them.

## What's next?

Continue exploring more advanced Copilot capabilities:

- Using GitHub Copilot for Azure - Learn how to interact with Azure resources directly from VS Code

## Resources

- [GitHub Copilot for MSSQL documentation][copilot-mssql]
- [SQL Server extension for VS Code][mssql-extension]
- [Quickstart: Use GitHub Copilot Agent Mode (Preview)][mssql-extension-agent-mode-quickstart]
- [Quickstart: Generate data for testing and mocking (Preview)][mssql-extension-data-testing-mocking-quickstart]
- [Writing queries with GitHub Copilot][copilot-sql-guide]
- [SQL Server T-SQL reference][tsql-reference]

[copilot-mssql]: https://learn.microsoft.com/en-us/sql/tools/visual-studio-code-extensions/github-copilot/overview?view=sql-server-ver17
[mssql-extension]: https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql
[mssql-extension-agent-mode-quickstart]: https://learn.microsoft.com/en-us/sql/tools/visual-studio-code-extensions/github-copilot/agent-mode?view=sql-server-ver17
[mssql-extension-data-testing-mocking-quickstart]: https://learn.microsoft.com/en-us/sql/tools/visual-studio-code-extensions/github-copilot/test-and-mocking-data-generator?view=sql-server-ver17
[copilot-sql-guide]: https://code.visualstudio.com/docs/copilot/copilot-chat
[tsql-reference]: https://docs.microsoft.com/en-us/sql/t-sql/language-reference
