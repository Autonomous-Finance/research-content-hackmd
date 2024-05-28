
# Dexi: Decentralized Autonomous Financial Data Network

## Introduction

Dexi is an application on the **AO network** that autonomously identifies, collects, and aggregates diverse financial data from various events within the AO network. This data spans *asset prices*, *token swaps*, *liquidity fluctuations*, and *token asset characteristics* like smart contract details. The platform is built on a network of autonomous agents and responsive components. Dexi’s terminal is hosted on the Arweave network, upholding its fully decentralized, permissionless nature.

Dexi caters primarily to two user segments: *end-users*, who access the platform through the web terminal at `dexi.arweave.dev`, and *AO apps*, which interact with Dexi by sending messages to leverage the collected data. A standout feature of the platform is its *data subscription service*. Processes on the AO network can subscribe to Dexi’s data feeds for a fee, receiving immediate alerts on updates like price adjustments. This feature negates the need for an external oracle, empowering any data-rich app on the AO network to act as an oracle and opening new revenue gross streams for developers.

### Exploring Dexi

In this article, we delve into the current state of Dexi and the functionalities available to users today. Later, we will explore the essential role and potential applications of the real-time data apps on the AO network. Additionally, we will offer concrete implementation examples demonstrating how developers can leverage this pattern in AO applications.

## Using Dexi 

### Terminal

To start using Dexi, visit the main page available under the ARNS domain `dexi`. You can access the terminal through various gateways such as [dexi.g8way.io](https://dexi.g8way.io/) or [dexi.arweave.dev](https://dexi.arweave.dev). A comprehensive list of gateaways can be found at [ViewBlock Gateways](https://viewblock.io/arweave/gateways).

Upon accessing the site, you will encounter the main discovery interface where tradable asset pairs and corresponding data are displayed. Here, you can connect your wallet and add assets to your favorites to create a personalized watchlist.

![Dexi Main Terminal](https://hackmd.io/_uploads/Hyc3suQVA.png)

After selecting an asset, you can delve deeper into specific details such as transaction data, price deviations, information about token holders, liquidity pools, LP tokens, total supply, volume, and more.

![Asset Details](https://hackmd.io/_uploads/rkm_b974R.png)

Additionally, Dexi allows for direct execution of swaps from its terminal, using a liquidity pool of your choice.

![Execute Swap](https://hackmd.io/_uploads/Hk_MfcXEA.png)

It's crucial to note that the entire setup is trustless. The terminal runs on Arweave and pulls data directly from the Dexi *aggregation agent*, making it completely censorship-resistant. All information is verifiable on-chain, safeguarding against manipulation attempts.


### Agent Subscription
To subscribe to the Data Agent in Dexi, you need to register your process with the Dexi agent and deposit a fee that will be used to cover the transaction costs. It's important to note that as the current AO Network is in testnet phase, there are no actual transaction costs, allowing developers to freely determine their own cost structure if they wish to apply this pattern. At Dexi, there is a requirement of a minimum of 1 AOCRED per process.

Currently, the Dexi agent network offers two service tiers: free and paid:

#### Free
- **Pull-based access**: Users can request data as needed.
- **Dry runs (read-only)**: Allows users to simulate transactions without actual execution.
- **Web UI**: Accessible terminal for casual browsing and data exploration.
- **Basic trading data**: Essential data for simple trading analysis.

#### Paid
- **Push-based (subscriptions)**: Data is automatically delivered based on specified criteria.
- **Periodic / Event-based**: Updates are sent at regular intervals or triggered by specific events.

Users can register an agent with Dexi by sending the following specific message format, which will add an entry to the subscriptions database:

```code
Send({
    Target = 'POJ5oyOzEnQf3Gm7yxVFOmWV5I-LfpAxIw_dYH1Kl-Y', 
    Action = 'Register-Process', 
    ['AMM-Process-Id'] = 'U3Yy3MQ41urYMvSmzHsaA4hJEDuvIm-TgXvSm-wz-X0', 
    ['Subscriber-Process-Id'] = <Process that should receive Signals>, 
    ['Owner-Id'] = <Subscriber Owner>
})
```
This structured setup ensures that developers and users can easily engage with Dexi’s services, utilizing the network’s capabilities for both basic and advanced data needs.

# Architecture

### Role of Dexi in the AO Network

Dexi's data agents facilitate the integration of financial data across various processes within the AO network. These agents are critical for:

- **Trading Agents:** They provide access to price and liquidity data necessary for formulating risk management strategies.
- **Exchanges:** They use Dexi as a reliable source for oracle-type price feeds.
- **Liquidity Pools and Swaps:** They utilize data to adjust their incentive structures dynamically.
- **On-chain LLMs:** They analyze data to enhance strategic decision-making.

Dexi is an AO-native application that addresses some of the common challenges faced by traditional blockchain systems, such as scalability and processing speed. By enabling advanced data processing tasks directly on the blockchain, Dexi supports the broader functionality of the AO computer. This support is crucial for developing sophisticated financial applications in the AgentFI space.


![Dexi Architecture](https://hackmd.io/_uploads/BkhWhb0mA.jpg)


## Implementation
## Key Functions of Dexi:

1. **Real-Time Data for Web Users:**
   Dexi provides immediate access to financial data and analytics on its website, helping traders and analysts make informed decisions quickly. This is facilitated via read-only dry run transactions.

2. **Data Subscription for Automated Agents:**
   Other applications or automated trading bots can subscribe to receive continuous data updates from Dexi, enabling them to respond instantly to market changes.


Unlike conventional blockchains, AO can execute arbitrary WASM code that is deterministic, enhancing scalability by avoiding a shared global state. Dexi capitalizes on this capability using the [**AOS-SQLite module**](https://github.com/permaweb/aos-sqlite), a WASM version of the SQLite database, allowing for on-chain database functionalities like aggregations, triggers, and complex queries. Dexi updates its internal state table by receiving proactive messages from AMMs as transactions occur. Specifically, it receives a copy of the Order-Confirmation message sent to users, which is used to update its data. This table undergoes aggregation to convert raw data into familiar trading indicators. 

Additionally, a cron process periodically updates key indicators and dispatches them to on-chain financial agents for timely trade execution.


## Hands on
At the core of the Dexi application is the transactions table.
```sql
CREATE TABLE IF NOT EXISTS amm_transactions (
    id TEXT PRIMARY KEY,
    source TEXT NOT NULL CHECK (source IN ('gateway', 'message')),
    block_height INTEGER NOT NULL,
    block_id TEXT,
    sender TEXT NOT NULL,
    created_at_ts INTEGER,
    to_token TEXT NOT NULL,
    from_token TEXT NOT NULL,
    from_quantity INT NOT NULL,
    to_quantity INT NOT NULL,
    fee INT INT NULL,
    amm_process TEXT NOT NULL
);
```
It serves as the single source of truth of all charts, metrics and signals. It does not feature any transformations and is as close as possible to the onchain data. Only the source column is added. Keep reading to find out what it refers to.


## Syncing
Having accurate data is paramount for Dexi, and it has two ways to achieve this:

### The input event
We are consuming a standardised event emitted by the AMM, called `Order-Confirmation`. This event is emitted on each successful swap. This event maps perfectly to our amm_transactions table.

### Pull Based
To backfill historical state Dexi can pull data in via an Oracle such as 0rbit. Dexi will request in an iterative fashion updates and consume these messages until it has caught up with the latest transactions. This works permisionlessly as gateway data is freely available.

### Push based
Dexi can also receive and verify messages from the registered AMMs. In that case updates do not suffer the delay from being published via the gateway. This allows for real time updates on the site, however it is opt in and needs to be configured during AMM instantiation.

### Validation
To prevent inputting invalid data into the database we leverage SQLite check constraints. A check constraint allows you to implement lightweight input validation on the DB layer. We combine it with application side validation using the [Lua Schema validation](https://github.com/erento/lua-schema-validation/blob/master/src/validation.lua) package.

## Constructing charts and metrics

### Intermediate View

Once data is persisted in the `amm_transactions` table we construct a view on top that handles the type conversions and adds a few additional features, such as determining wheteher the current transaction is a buy or a sell. 

```sql
CREATE TABLE IF NOT EXISTS amm_registry (
    amm_process TEXT PRIMARY KEY,
    amm_name TEXT NOT NULL,
    amm_token0 TEXT NOT NULL,
    amm_token1 TEXT NOT NULL,
    amm_discovered_at_ts INTEGER
);
```

It also combines data from the AMM registry to add in some relevant meta information. You may ask, why not just push this logic into the underlying table? As long as you keep these transformations lightweight enough using a view here gives you much more flexibility without resorting to complex migrations. Want to add a column? Just swap out the view.


```sql
CREATE VIEW amm_transactions_view AS
SELECT
  id,
  source,
  block_height,
  block_id,
  sender,
  created_at_ts,
  to_token,
  from_token,
  from_quantity,
  to_quantity,
  fee,
  amm_process,
  CASE WHEN to_token = amm_token1 THEN 1 ELSE 0 END AS is_buy,
  ROUND(CASE 
    WHEN from_quantity > 0 AND to_quantity > 0 THEN
      CASE 
        WHEN to_token = amm_token1 THEN from_quantity * 1.0 / to_quantity
        ELSE to_quantity * 1.0 / from_quantity
      END
    ELSE NULL
  END, 12) AS price,
  CASE
    WHEN to_token = amm_token1 THEN from_quantity
    ELSE to_quantity
  END AS volume
FROM amm_transactions
LEFT JOIN amm_registry USING (amm_process)
```

## Constructing charts and metrics

Now that you have this view constructing metrics is very easy.
Querying transactions and volume is as easy as this
```sql
SELECT
  amm_process,
  COUNT(*) AS transactions,
  SUM(volume) AS volume
FROM amm_transactions_view
WHERE created_at_ts >= :now - 86400
GROUP BY 1
```

Refer to candles.lua for an example of how to retreive candles from the present data. The Dexi process exposes many of these queries via handlers, you can see them in action on https://dexi.g8way.io/
or find their description in the README.

## Dispatching Signals

Now that we have demonstrated how we can go from indexing onchain data, to structuring to constructing signals lets pivot into dispatching signals.

## Payment Model
Dexi's payment model demonstrates the AO network's flexibility. Read-only offchain requests are handled by a subsidized CU, serving non-commercial users who browse information for potential purchases. These nodes can be locally operated, allowing anyone to verify results independently.

For onchain financial agents who require enhanced economic security due to immediate actions based on Dexi's signals, the model differs. Trigger dispatches operate on the AO mainnet, secured by its full economic safeguards. This service incurs gas costs, which are covered by a subscription fee. Financial agent operators select the signals they need, and are provided with an estimate of the gas expenses plus Dexi fees for a specified period. Once the payment is completed, the owner registers their bots with Dexi to periodically receive the necessary data.


### Funding
Before Dexi dispatches messages the user has to add funds for this particular process. At the moment a minimum balance of 1 AOCRED is necessary to start receiving messages. Funding is added on a per user basis and can be shared across many processes.

Tracking funds for a user is achieved via a SQLite balances table:
```lua
function sqlschema.updateBalance(ownerId, tokenId, amount, isCredit)
  local stmt = db:prepare[[
    INSERT INTO balances (owner, token_id, balance)
    VALUES (:owner_id, :token_id, :amount)
    ON CONFLICT(owner) DO UPDATE SET
      balance = CASE 
        WHEN :is_credit THEN balances.balance + :amount
        ELSE balances.balance - :amount
      END
    WHERE balances.token_id = :token_id;
  ]]
  if not stmt then
    error("Failed to prepare SQL statement for updating balance: " .. db:errmsg())
  end
  stmt:bind_names({
    owner_id = ownerId,
    token_id = tokenId,
    amount = math.abs(amount),  -- Ensure amount is positive
    is_credit = isCredit
  })
  local result, err = stmt:step()
  stmt:finalize()
  if err then
    error("Error updating balance: " .. db:errmsg())
  end
end
```

### Constructing Indicators

Building indicators is a direct process. Simply retrieve data from SQLite, aggregate it according to your preferred frequency, and apply custom Lua logic to compute the desired indicators. For examples of how to calculate common indicators using Lua, refer to the `indicators.lua` script.


### The Cron
Dexi receives a cron tick every minute, however currently it dispatches signals every hour. It does this by checking the current timestamp of the cron message and triggers the dispatch logic once a new hour begins.
Every new hour it: 
- Loops through all registered AMMs
- Checks all subscribers that are funded
- Calculates the indicators
- Dispatches the message
```lua
    local ammsStmt = db:prepare([[
      SELECT amm_process, amm_discovered_at_ts 
      FROM amm_registry
    ]])
    if not ammsStmt then
      error("Err: " .. db:errmsg())
    end
    
    local oneWeekAgo = now - (7 * 24 * 60 * 60)
    
    for row in ammsStmt:nrows() do
      local ammProcessId = row.amm_process
      local discoveredAt = row.amm_discovered_at_ts
      
      local startTimestamp = math.max(discoveredAt, oneWeekAgo)
      indicators.dispatchIndicatorsMessage(ammProcessId, startTimestamp, now)
    end
    ammsStmt:finalize()
    print('Dispatched indicators for all AMMs')
```


The subscribing agent will now receive a message every hour with the Action `IndicatorsUpdate` that contains indicators as the Data payload with the AMM tag specifying the source AMM.
The data field contains an array of objects with indicators. It goes back 7 days. The object looks like this.
`indicatorUpdate = json.decode(msg)`
```
>indicatorUpdate[1]
{
   date = "2024-04-16",
   sma200 = 10,
   sma100 = 10,
   open = 10,
   sma50 = 10,
   sma150 = 10,
   volume = 220,
   close = 10,
   low = 6.666666666667,
   high = 10,
   sma20 = 10,
   sma10 = 10
}
```
It can then implement bespoke trading logic, eg detecting a golden cross

```lua
local todaysIndicatorUpdate = indicatorUpdate[1]
function detectGoldenCross(indicatorUpdate)
  local goldenCross = false
  
  if indicatorUpdate.sma50 > indicatorUpdate.sma200 then
    goldenCross = true
  end
  
  return goldenCross
end

detectGoldenCross(todaysIndicatorUpdate)
```

## Building & Deploying
While AOS comes with a built in loader that can resolve modules, we opt for using Lua Amalg. This gives us an amalgamation file that can be easily Eval'd in one go with AOconnect. We use this in conjunction with aoform, a lightweight utility to deploy AO processes. You can check it out here:
https://github.com/Autonomous-Finance/aoform

Deployment (build.sh):
```
/opt/homebrew/bin/luacheck process.lua schemas.lua sqlschema.lua intervals.lua candles.lua stats.lua validation.lua indicators.lua

/opt/homebrew/bin/amalg.lua -s process.lua -o build/output.lua sqlschema intervals schemas validation candles stats indicators

npx aoform apply
```
In our experience amalagamations work more reliably than the aos loader (for now) and it also has the nice property of beign able to deploy with just one command.



# Conclusion

In conclusion, Dexi exemplifies a robust implementation of a decentralized application leveraging the AO blockchain's unique capabilities. By integrating SQLite within a blockchain environment, Dexi enables sophisticated data analysis directly on-chain, supporting real-time decision-making in DeFi spaces. The application not only addresses the efficiency and scalability issues commonly associated with blockchain applications but also showcases an innovative approach to on-chain data management and interaction.

As the blockchain and DeFi landscapes continue to evolve, applications like Dexi will be pivotal in shaping the future of financial transactions and strategy implementation in decentralized networks. The continuous refinement and adaptation of Dexi will further solidify its role as a critical tool for traders and financial analysts in the blockchain ecosystem, driving forward the fusion of technology and finance in unprecedented ways.


Autonomous AMM D