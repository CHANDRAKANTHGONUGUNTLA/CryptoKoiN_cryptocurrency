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

Below is the updated relational diagram illustrating the relationships between tables:

![Relational Diagram](./diagrams/relational_diagram.png)

### ER Diagram (Optional)

![ER Diagram](./diagrams/er_diagram.png)

## Credentials per User Role

| Role       | Username     | Password   |
|------------|--------------|------------|
| Customer   | srikanth        | 123456    |
| Admin      | chandu       | 123456   |

## SQL Queries

### Transactional Queries:
1. **Insert a New User**:
   ```sql
   INSERT INTO Users (username, password_hash, email, created_at, role) 
   VALUES ('user1', HASHBYTES('SHA2_256', 'pass123'), 'user1@example.com', GETDATE(), 'customer');
   ```

2. **Record a Transaction**:
   ```sql
   INSERT INTO Transaction_History (transaction_type, quantity, price_at_transaction, transaction_timestamp, crypto_id, user_id, total_amount) 
   VALUES ('buy', 2, 45000, GETDATE(), 1, 1, 90000);
   ```

3. **Update Wallet Balance**:
   ```sql
   UPDATE Wallet SET balance = balance - 90000 WHERE user_id = 1;
   ```

### Analytical Queries:
1. **Top Cryptocurrencies by Market Cap**:
   ```sql
   SELECT name, symbol, market_cap FROM Cryptocurrency 
   ORDER BY market_cap DESC;
   ```

2. **Total Revenue by Day**:
   ```sql
   SELECT CAST(transaction_timestamp AS DATE) AS transaction_date, SUM(total_amount) AS daily_revenue 
   FROM Transaction_History 
   GROUP BY CAST(transaction_timestamp AS DATE);
   ```

3. **User Activity Log Count**:
   ```sql
   SELECT user_id, COUNT(*) AS activity_count 
   FROM User_Activity 
   GROUP BY user_id;
   ```

## Features

### User Interface:
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

### Core Functionality:
- **Data Handling**:
  - Secure storage of user and transaction data using SQL Server.
  - Data updates from CoinAPI.
- **Visualizations**:
  - Graphical representations of market trends using candlestick charts.

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
   - Obtain an API key from [CoinAPI](https://www.coinapi.io/).
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

### Admin Endpoints:
- `/admin_mainmenu` - Admin dashboard.
- `/admin_mainmenu/user_management` - Manage users.
- `/admin_mainmenu/transactions` - View transactions.
- `/admin_mainmenu/wallet_transactions` - View wallet transactions.
- `/admin_mainmenu/market` - Manage market data.
- `/admin_coin_details/<coin_id>` - View coin details with candlestick graph.


