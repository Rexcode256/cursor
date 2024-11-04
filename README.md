  import time
import random

class XAUUSDTradingBot:
    def __init__(self):
        self.balance = 100000  # Starting balance in USD
        self.position = 0  # Current position in ounces of gold
        self.symbol = "XAU/USD"
        self.support_level = 1800  # Initial support level
        self.resistance_level = 1900  # Initial resistance level
        self.price_history = []

    def get_market_price(self):
        # In a real bot, this would fetch live market data for XAU/USD
        # For this example, we'll simulate a price between 1750 and 1950
        return random.uniform(1750, 1950)

    def update_support_resistance(self):
        # In a real bot, this would use more sophisticated methods to determine support and resistance
        if len(self.price_history) > 20:
            self.support_level = min(self.price_history[-20:])
            self.resistance_level = max(self.price_history[-20:])

    def analyze_market(self, price):
        self.price_history.append(price)
        self.update_support_resistance()

        # Buy if price is at support level, sell if at resistance level
        if price <= self.support_level and self.balance > 0:
            return 'buy'
        elif price >= self.resistance_level and self.position > 0:
            return 'sell'
        else:
            return 'hold'

    def execute_trade(self, action, price):
        if action == 'buy':
            ounces_to_buy = self.balance // price
            cost = ounces_to_buy * price
            self.balance -= cost
            self.position += ounces_to_buy
            print(f"Bought {ounces_to_buy:.2f} ounces of gold at ${price:.2f}")
        elif action == 'sell':
            self.balance += self.position * price
            print(f"Sold {self.position:.2f} ounces of gold at ${price:.2f}")
            self.position = 0

    def run(self):
        while True:
            price = self.get_market_price()
            action = self.analyze_market(price)
            self.execute_trade(action, price)
            print(f"Balance: ${self.balance:.2f}, Position: {self.position:.2f} ounces")
            print(f"Support: ${self.support_level:.2f}, Resistance: ${self.resistance_level:.2f}")
            time.sleep(1)  # Wait for 1 second before next iteration

# Create and run the bot
bot = XAUUSDTradingBot()
bot.run()
