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
### User Interface:
- **User MainMenu**:
  -  # -------------- M2 ------------------

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


