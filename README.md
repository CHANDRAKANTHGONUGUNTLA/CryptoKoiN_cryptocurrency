# CryptoKoiN
CryptoKoiN is a cryptocurrency web application that provides user-friendly features for managing cryptocurrency assets, tracking transactions, and visualizing market trends. The application supports both user and admin interfaces with secure functionality and robust data handling using SQL Server and Flask.

## Project Details

### Group Members:
- Gonuguntla Chandrakanth Naidu

### Narrative:
CryptoKoiN was developed to simplify cryptocurrency management for users while providing admins with tools to monitor and analyze transactions. The application integrates data from CoinGeckoAPI, ensuring real-time market updates and facilitating informed decision-making.

#### Primary Use Cases:
- Users can buy, sell, and track cryptocurrency.
- Admins can manage users and analyze transaction trends.

#### User Roles:
- **Customer**: Engage in cryptocurrency trading and monitor personal transactions.
- **Admin**: Oversee user activity, manage market data, and analyze revenue trends.

#### Main Purpose of Each User Role:
- **Customer**: Facilitate secure and efficient cryptocurrency management.
- **Admin**: Ensure system integrity and monitor overall application performance.

## Relational Diagram
# -------------- M1 ------------------

Below is the updated relational diagram illustrating the relationships between tables:

![Relational Diagram](./diagrams/relational_diagram.png)

### ER Diagram (Optional)

![ER Diagram](./diagrams/er_diagram.png)

## Credentials per User Role

| Role       | Username     | Password   |
|------------|--------------|------------|
| Customer   | srikanth        | 123456    |
| Admin      | chandu       | 123456   |



## Features

- **Homepage**
  - The homepage is the entry point of the application, showcasing the following buttons for navigation:
     - **Login**: For both users and admins to log in.
     - **Create Account**: Allows users to register.
     - **Forgot Password**: Users can reset their passwords using their registered email address.

### User Interface:
- **User Main Menu**:
  - Central hub for users to manage their cryptocurrency portfolio and transactions.
  - Features include:
    - **Portfolio Overview**: Visualize cumulative cryptocurrency holdings and investment trends over time.
    - **Profitable Coins (24 Hours)**: Identify top-performing cryptocurrencies in the last 24 hours.
    - **Market Data**: Access real-time price, volume, and trend information for various cryptocurrencies.
  - Designed for seamless navigation and quick access to critical investment insights.

- **Cryptocurrency Market**:
  - Detailed insights on cryptocurrencies, including market trends, historical data, and performance metrics.
  - Highlight top gainers and losers over various timeframes (e.g., daily, weekly).
  - Real-time data visualization with sortable and filterable tables.
- **Trading**:
  - Purchase cryptocurrencies directly using wallet balance.
  - Real-time price tracking and wallet deductions.
  - Confirmation prompts and transaction summaries for user verification.
  - Instant sell options with real-time calculations.
  - Wallet balance updates post transaction.
    
- **Currency Detailing**:
  - Access price trends and volatility reports for individual cryptocurrencies.
  - Time-series analysis over multiple durations (daily, monthly, yearly).
  - Interactive Charts: Utilize candlestick and line charts for in-depth trend analysis.
  - Adjustable time intervals for personalized insights.
- **Wallet Management**:
  - View wallet balance.
  - Withdraw funds to a bank account.
  - Deposit funds from a bank account/cards.
  - View wallet transaction history.
- **Cryptocurrency Investments**:
  - View details of owned cryptocurrencies.
  - Analyze profit and loss.
- **Transaction Management**:
  - View a complete history of cryptocurrency transactions.
- **Profile Management**:
  - View and manage personal account details.
- **Rewards Program**:
  - Incentivizes users to engage more with the platform through rewards.
  - Features include:
    - **Referral Program**: Earn rewards for inviting new users to join the platform.
    - **New Cryptocurrency Launch Rewards**: Exclusive bonuses for participating in the launch of new cryptocurrencies.
  - Encourages user participation and enhances platform loyalty.


### Admin Interface:
- **Dashboard**:
  - User growth statistics.
  - Daily transaction revenue.
  - Top users by transaction volume.
  - Cryptocurrency distribution analysis.
- **User Management**:
  - View and manage user accounts.
- **Market Management**:
  - View and manage cryptocurrency data.
- **Transaction Analysis**:
  - View and filter user and wallet transactions.


## Database Query Management Using `user.py`, `base.py`, and `cryptokoin_api.py`

In this architecture, three key Python files are responsible for managing database queries and external API interactions: `user.py`, `base.py`, and `cryptokoin_api.py`. These files streamline and centralize database operations and provide secure handling of queries while interacting with external services like CoinGecko for cryptocurrency data.

1.  **user.py**

    -   **Purpose**: Handles user-related operations like authentication, registration, and password validation.
    -   **Key Features**:
        -   `hashPassword`: Secures passwords using a hashing algorithm (MD5 with salt).
        -   `verify_new` & `verify_update`: Validates new and updated user data (e.g., username, email, password matching).
        -   `tryLogin`: Handles user login by validating credentials against the database.
          
2.  **baseObject.py**

    -   **Purpose**: Manages the basic database operations such as connecting to the database, executing CRUD operations, and handling database errors.
    -   **Key Features**:
        -   `setup`: Initializes the database connection and prepares the object for CRUD operations.
        -   `insert`, `update`, `deleteById`: Simplifies data insertion, updating, and deletion processes.
        -   `getAll`, `getById`, `getByField`: Fetches records from the database based on various conditions.
          
3.  **cryptokoin_apis.py**

    -   **Purpose**: Provides functions to interact with cryptocurrency APIs (like CoinGecko and Yahoo Finance) and manage cryptocurrency-related data.
    -   **Key Features**:
        -   `fetch_current_prices`: Retrieves real-time cryptocurrency prices from the CoinGecko API.
        -   `calculate_profit_loss`: Computes the profit or loss based on transaction history and current coin prices.
        -   `candle_stick_graph`: Generates a candlestick chart using historical data for a specific cryptocurrency.

This architecture ensures a modular, secure, and maintainable approach for handling user and database interactions, while also offering flexibility for integrating external APIs.


 # Title -------------- M3 ------------------
## User MainMenu Highights

### **Portfolio Overview**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  Displays a graphical representation of the user's cryptocurrency holdings and tracks the total portfolio value over time.

- **Purpose of the Graph**:  
  - Track cumulative cryptocurrency holdings day-by-day.  
  - Help users analyze their investment growth or decline.  
  - Enable users to make informed decisions on future trades.

- **SQL Code**:
  ```sql
  SELECT date, 
         SUM(value) AS cumulative_portfolio_value
  FROM user_holdings
  WHERE user_id = ?
  GROUP BY date
  ORDER BY date;```
- **Explanation**: 
  - Retrieves daily cryptocurrency holdings for the user.
  - Uses SUM(value) to calculate the cumulative portfolio value for each date.
  - Data is ordered by date to visualize portfolio trends effectively.
- **Insights Users Gain**:
  - Understand how their investments perform over time.
  - Identify profitable or unprofitable trends in specific periods.

### **Profitable Coins (24 Hours)**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  Highlights the cryptocurrencies with the highest profit percentages in the past 24 hours.
- **Purpose of the Graph**:  
  - Identify top-performing cryptocurrencies.
  - Assist users in making buy/sell decisions based on recent performance.

- **SQL Code**:
  ```sql
    SELECT coin_id, name, ((current_price - opening_price) / opening_price) * 100 
    AS profit_percentage
    FROM coin_market_data
    WHERE timestamp >= NOW() - INTERVAL 1 DAY
    ORDER BY profit_percentage DESC
    LIMIT 5;
  ```

- **Explanation**:
  - Fetches the top 5 cryptocurrencies by profit percentage in the last 24 hours.
  - Uses the formula ((current_price - opening_price) / opening_price) * 100 to calculate the profit percentage.
  - Orders the results by profit_percentage in descending order to show the highest gainers.
- **Insights Users Gain**:
  - Quickly identify profitable cryptocurrencies to take action.
  - Spot opportunities to buy/sell based on the latest market performance.

## Admin MainMenu Highights
### **User Growth Trends**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  Displays a graph of user registrations, showing how the platform is growing over time.

- **Purpose of the Graph**:  
  - Analyze user growth trends over the last 12 months.
  - Understand the impact of campaigns and marketing strategies.

- **SQL Code**:
  ```sql
      SELECT MONTH(created_at) AS month, 
             COUNT(user_id) AS new_users
      FROM users
      WHERE created_at >= DATE_SUB(NOW(), INTERVAL 12 MONTH)
      GROUP BY MONTH(created_at)
      ORDER BY month;
  ```
- **Explanation**:
  - Retrieves the number of new users registered each day for the past year.
  - Groups data by month to create a clear trend of user growth.

### **Total Users**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  Displays the total number of users registered on the platform.
  
- **SQL Code**:
  ```sql
       SELECT COUNT(*) AS total_users
       FROM users;
  ```
- **Explanation**:
  - Counts all users in the users table to provide the total user count.
  - Useful for monitoring the platform's user base.


### **Total Revenue from Transaction Fees**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  Highlights the total revenue generated from transaction fees.
  
- **SQL Code**:
  ```sql
       SELECT SUM(transaction_fee) AS total_revenue
       FROM transactions
       WHERE timestamp >= DATE_SUB(NOW(), INTERVAL 1 MONTH);
  ```
- **Explanation**:
  - Aggregates transaction fees for all transactions.
  - Provides insight into the platform's revenue performance.


### **Daily Revenue with Transaction Breakdown**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  Displays a breakdown of daily revenue trends based on transaction types (buy/sell).
  
- **SQL Code**:
  ```sql
      SELECT DATE(timestamp) AS day, 
             SUM(transaction_fee) AS daily_revenue, 
             transaction_type
      FROM transactions
      GROUP BY day, transaction_type
      ORDER BY day;  
  ```
- **Explanation**:
  - Groups revenue data by transaction type and day.
  - Helps track which transaction types contribute the most to daily revenue.

### **Top Cryptocurrencies by Transaction Volume (Pie Chart)**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  A pie chart showing the most traded cryptocurrencies based on transaction volume.
  
- **SQL Code**:
  ```sql
    SELECT coin_id, name, COUNT(*) AS transaction_count
    FROM transactions
    GROUP BY coin_id
    ORDER BY transaction_count DESC
    LIMIT 10;
  ```
- **Explanation**:
  - Fetches the top 10 cryptocurrencies by the number of transactions.
  - Groups by coin_id to count the transactions per cryptocurrency.


### **Top Users by Transaction Value**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  Lists the top users based on the total value of their transactions over the time.
  
- **SQL Code**:
  ```sql
    SELECT user_id, SUM(transaction_value) AS total_transaction_value
    FROM transactions
    WHERE timestamp >= DATE_SUB(NOW(), INTERVAL 1 MONTH)
    GROUP BY user_id
    ORDER BY total_transaction_value DESC
    LIMIT 5;
  ```
- **Explanation**:
  - Aggregates transaction values for each user to identify the highest contributors.
  - Orders by total_transaction_value in descending order to highlight top users.


## Detail Feature Explanation
### **Homepage**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  The homepage acts as the starting point for the CryptoKoin platform, offering navigation to key functionalities like login, sign-up, and password reset.

- **Purpose**:
  - Provide clear navigation options for new and returning users.
  - Serve as the entry point for account authentication and recovery.

- **Backend Flow**:
  - No direct SQL interaction here, as this page only provides navigation links.
    
- **Next Actions**:
  - Users select **Log In**, **Sign Up**, or **Reset Password** to proceed to specific functionalities.

### **Login**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  This page allows users to authenticate themselves by entering a username and password.
- **Purpose**:
  - Validate user credentials against stored data.
  - Redirect valid users to their respective dashboard (user or admin).
  - Start user session tracking for activity monitoring.
  
- **SQL Code**:
  ```sql
  
  ```
- **Explanation**:
  - The `username` and `password` fields submitted by the user are captured via `request.form`.
  - The SQL query checks if a matching username-password pair exists in the users table.
  - If a match is found:
    - User session starts.
    - User activity timer begins (30-minute inactivity logout threshold).
    - Redirect:
        - If user_role == 'admin', the user is redirected to the Admin Dashboard.
        - Otherwise, the user is redirected to the User Dashboard.
  - If no match is found, an error message is returned.

- **Insights Users Gain**:
  - Seamless access to their accounts upon valid login.
  - Role-based redirection ensures proper navigation.

### **Create Account**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  This page enables new users to sign up by entering required details like username, email, and password.
- **Purpose**:
  - Add a new user to the platform's database.
  - Ensure input validation and prevent duplicate accounts.
- **SQL Code**:
  ```sql
  
  ```
- **Explanation**:
  - User input (username, email, password, and optional referral_code) is captured via request.form.
  - A check is performed to ensure the username or email doesn't already exist.
  - If validation passes, the user is added to the users table.
  - On successful creation, the user is redirected to the **Login Page**.

- **Insights Users Gain**:
  - Confidence in a secure and validated sign-up process.

### **Reset Password**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  Allows users to reset their password by verifying their username and email.

- **Purpose**:
  - Update a user’s password securely.
  - Redirect the user to the login page after successful reset.
- **SQL Code**:
  ```sql
  
  ```
- **Explanation**:
  - User input (username, email, and new_password) is captured via request.form.
  - The SQL query verifies the existence of the username-email combination before updating the password.
  - On successful reset, the user is redirected to the **Login Page**.

- **Insights Users Gain**:
  - Assurance of a secure password recovery process.


### **Investment Page**
![Relational Diagram](./diagrams/relational_diagram.png)

- **Description**:  
  The investment page displays the user's current cryptocurrency portfolio, showing an overview of the coins they hold, quantities, and their total portfolio value.

- **Purpose of the Template**:
  To provide users with an organized view of their investments, enabling them to see the performance of their portfolio at a glance.

- **SQL Code**:
  ```sql
    SELECT coin_id, name, COUNT(*) AS transaction_count
    FROM transactions
    GROUP BY coin_id
    ORDER BY transaction_count DESC
    LIMIT 10;
  ```
- **Explanation**:
  - **Portfolio Value**: Highlights the total value of the user's portfolio, dynamically calculated using the current balance of all investments.
  - **Investment Table**:
    - **Icon**: Displays the coin's logo.
    - **Coin Name**: The name of the cryptocurrency.
    - **Quantity**: The number of coins held.
    - **Current Price**: Shows the live price of the cryptocurrency.
    - **Current Value**: The total value of holdings for that coin, calculated as Quantity x Current Price.

- **Insights Users Gain**:
  - Real-time updates on their portfolio’s worth.
  - Clear breakdown of their holdings by coin, aiding in strategic decision-making.

- **Notes**:
  - Option to link to a "Details" or "Market" page for deeper insights into each coin.
  - A "Trade" button can be added for quick actions like buying or selling coins.

### **Market Page**

- **Description**:
  The market page lists cryptocurrencies with key data such as price, 24-hour changes, and a link to detailed views.

- **Purpose of the Template**:
  To provide users with a marketplace to explore various cryptocurrencies and their performance metrics.

- **SQL Code**:
  ```sql
    SELECT coin_id, name, COUNT(*) AS transaction_count
    FROM transactions
    GROUP BY coin_id
    ORDER BY transaction_count DESC
    LIMIT 10;
  ```
- **Explanation**:
    - **Crypto Table**:
    - **Logo**: Displays the coin’s logo.
    - **Symbol and Name**: Shows the ticker and name of the coin.
    - **Price**: Real-time price of the cryptocurrency in USD.
    - **24h Change (% and $)**: Indicates percentage and absolute change in value over 24 hours.
    - **Details**: Links to the details page for in-depth analysis.

- **Insights Users Gain**:
  - Real-time performance metrics of cryptocurrencies.
  - Links to detailed data for better-informed trading decisions.

- **Notes**:
  - Provide filters for sorting (e.g., by price or market cap).
  - Option for a "Trade" button to directly transition to trading from this page.

### **Details Page**

- **Description**:
  The details page gives a comprehensive view of a specific cryptocurrency,       including its price trends and market statistics.

- **Purpose of the Template**:
  To assist users in analyzing a particular cryptocurrency deeply before making trading decisions.
  
- **SQL Code**:
  ```sql
    SELECT coin_id, name, COUNT(*) AS transaction_count
    FROM transactions
    GROUP BY coin_id
    ORDER BY transaction_count DESC
    LIMIT 10;
  ```
- **Explanation**:
  - **Graph Container**: Displays a candlestick chart for price movements.
  - **Coin Details**:
    - Includes data like symbol, current price, market cap, 24-hour high/low, and all-time highs.
    - Highlights percentage changes in different time frames.
  - **Trade Button**: Redirects users to the trade page for buying or selling the cryptocurrency.

- **Insights Users Gain**:
  - Price trends for the coin to make informed buy/sell decisions.
  - Market statistics like rank and cap to gauge the coin's popularity and stability.

- **Notes**:
  - Ensure seamless navigation between this and the trade page for user convenience.
  - Add historical data for advanced analysis.

### **Transactions Page**:
- **Description**: 
  The transactions page lists all trades performed by the user, including buys and sells.
- **Purpose of the Template**:
  To provide users with a history of their transactions, helping them track their activity and analyze past trades.

- **SQL Code**:
  ```sql
    SELECT coin_id, name, COUNT(*) AS transaction_count
    FROM transactions
    GROUP BY coin_id
    ORDER BY transaction_count DESC
    LIMIT 10;
  ```
- **Explanation**:
  - **Transaction Table**:
    - **Icon and Coin Name**: Indicates the traded cryptocurrency.
    - **Order Type**: Specifies whether it was a buy or sell order.
    - **Quantity and Value**: Shows the number of coins traded and the transaction value.
    - **Date**: Timestamp of the transaction.

- **Insights Users Gain**:
  - Detailed overview of their trade history.
  - Ability to analyze trading patterns and performance.

- **Notes**:
  - Include a note: "Similar to Transactions, there’s also a page for Wallet Transactions to view previous deposit/withdrawal history."
  - Add filters for sorting by date or transaction type.

 ### **Wallet Deposit Page**:

- **Description**:
  This page allows users to deposit funds into their wallet using credit card details.

- **Purpose of the Template**:
  To enable users to seamlessly add funds to their wallets for trading or investment purposes.
  
- **SQL Code**:
  ```sql
    SELECT coin_id, name, COUNT(*) AS transaction_count
    FROM transactions
    GROUP BY coin_id
    ORDER BY transaction_count DESC
    LIMIT 10;
  ```
- **Explanation**:
  - **Deposit Form**:
    - Collects information like deposit amount, cardholder name, card number, expiry date, and CVV.
    - Ensures secure submission of financial data.
      
- **Insights Users Gain**:
  - A transparent and secure way to recharge their wallets.
  - Real-time updates on wallet balance post-deposit.
- **Notes**:
  -  "Just like Wallet Deposit, there’s also a page for Wallet Withdrawal to transfer funds back to the user’s bank account."

### **User Profile Page**

- **Description**:
  The user profile page displays user-specific information, including account details and wallet balance.

- **Purpose of the Template**:
  To provide users with a centralized view of their account information.

- **SQL Code**:
  ```sql
    SELECT coin_id, name, COUNT(*) AS transaction_count
    FROM transactions
    GROUP BY coin_id
    ORDER BY transaction_count DESC
    LIMIT 10;
  ```
- **Explanation**:

  - **Profile Data**:
    - Displays username, email, role, membership duration, and wallet balance.
    - Includes action links for password reset and wallet recharge.
- **Insights Users Gain**:
  - A snapshot of their account and financial standing.
  - Easy access to essential account actions.
- **Notes** 
  - Include links for resetting passwords and recharging wallets.
  - Provide an option for updating profile information or security settings.


### **User Login Activity and Session Management**

1.  **Tracking User Login and Session Duration**: Captures login time, session start, and session duration in the database for security and activity tracking.
2.  **Session Timeout Management**: Ensures user sessions automatically expire after 30 minutes of inactivity, preventing unauthorized access.
3.  **Login Error Handling**: Logs failed login attempts and provides clear feedback to users in case of incorrect credentials or session timeouts.
4.  **Database Structure for User Activity**: Utilizes the `user_activity` table to store detailed user login/logout information and track session lengths.


### Error Handling and User Feedback


1.  **Centralized Error Handling Across the Application**

    -   Errors are detected at every stage of user interaction, from login to transactions. The system checks for invalid input formats, insufficient funds, or unauthorized access at each point, ensuring that issues are caught early.
2.  **Actionable Feedback for Users**

    -   When an error occurs, the system provides clear, user-friendly messages guiding users on how to resolve the issue. For instance, if the user enters an incorrect email format, the system will show "Please enter a valid email address," helping users correct their mistakes.
3.  **Error Logging and Prevention Measures**

    -   All error occurrences are logged for monitoring and future improvements. In case of failed transactions or invalid actions (like attempting to sell more than holdings), the system ensures no data corruption or inconsistency occurs, and errors are logged for review and resolution.
4.  **Potential Errors and Examples**

    -   Errors may include login failures due to incorrect credentials, insufficient balance during transactions, unauthorized access to restricted pages, or session timeouts. These errors prevent users from performing unauthorized actions or losing data integrity. Examples: Invalid email format, selling more than holdings, session timeout.

### Core Functionality:
- **Data Handling**:
  - Secure storage of user and transaction data using SQL Server.
  - Data updates from CoinGeckoAPI.
- **Visualizations**:
  - Graphical representations of market trends using candlestick charts.


## SQL Queries

### Transactional Queries:
1. **Insert a New User**:

2. **Record a Transaction**:


3. **Update Wallet Balance**:


### Analytical Queries:
1. **Top Cryptocurrencies by Market Cap**:

2. **Total Revenue by Day**:

3. **User Activity Log Count**:
## Setup Instructions

### Prerequisites:
- Python (3.8 or higher)
- Flask
- SQL Server
- CoinAPI key

### Installation:
1. Clone this repository:
   ```bash
   git clone <repository_url>
   cd CryptoKoiN
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Configure the database:
   - Set up a SQL Server database.
   - Update database connection details in the `config.py` file.
4. Configure CoinAPI:
   - Obtain an API key from [CoinAPI](https://www.coingeckoapi.io/).
   - Update the API key in the `cryptokoin_api.py` file.

### Running the Application:
1. Start the Flask development server:
   ```bash
   python app.py
   ```
2. Access the application in your browser at `http://127.0.0.1:5000`.

## Folder Structure
- `templates/`: HTML templates for the application.
- `static/`: CSS, JavaScript, and image files.
- `app.py`: Main Flask application file.
- `config.py`: Configuration file for database and API keys.
- `cryptokoin_api.py`: API integration and data handling.

## APIs and Endpoints

### User Endpoints:

- `/mainmenu` - User main menu.
- `/mainmenu/wallet/withdraw` - Wallet withdrawal functionality.
- `/mainmenu/wallet/transactions` - View wallet transaction history.
- `/mainmenu/investments` - View investments and profit/loss analysis.
- `/mainmenu/transactions` - View transaction history.
- `/mainmenu/account/profile` - View user profile.
- `/mainmenu/market` - Manage market data.
- `/details/<coin_id>` - View coin details with candlestick graph.
- `/mainmenu/rewards` - Referral Program and Launching a new cryptocurrency

### Admin Endpoints:
- `/admin_mainmenu` - Admin dashboard.
- `/admin_mainmenu/user_management` - Manage users.
- `/admin_mainmenu/transactions` - View transactions.
- `/admin_mainmenu/wallet_transactions` - View wallet transactions.
- `/admin_mainmenu/market` - Manage market data.
- `/admin_coin_details/<coin_id>` - View coin details with candlestick graph.


