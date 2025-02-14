import random
import time

class VirtualGame:
    def __init__(self):
        self.balance = 1000  # Starting virtual coins
        self.history = []
        self.colors = ['🔴 Red', '🟢 Green', '🔵 Blue', '🟡 Yellow']
        self.multipliers = {
            '🔴 Red': 1.5,
            '🟢 Green': 2.0,
            '🔵 Blue': 3.0,
            '🟡 Yellow': 5.0
        }

    def display_rules(self):
        print("\n🎮 Game Rules:")
        print(f"1. Bet virtual coins on colors")
        print(f"2. Win multipliers: {self.multipliers}")
        print(f"3. Starting balance: {self.balance} coins")
        print("4. No real money involved\n")

    def spin_wheel(self):
        print("\nSpinning...")
        time.sleep(2)
        return random.choice(self.colors)

    def place_bet(self):
        try:
            print(f"\nAvailable Colors: {', '.join(self.colors)}")
            color_choice = input("Choose color: ").strip().title()
            bet_amount = int(input(f"Enter bet amount (1-{self.balance}): "))
            
            if bet_amount > self.balance:
                print("❗ Insufficient balance!")
                return None
                
            return color_choice, bet_amount
        except ValueError:
            print("❗ Invalid input!")
            return None

    def update_balance(self, result, color_choice, bet_amount):
        if result in color_choice:
            win = int(bet_amount * self.multipliers.get(result, 1))
            self.balance += win
            self.history.append(f"Won {win} on {result}")
            return win
        else:
            self.balance -= bet_amount
            self.history.append(f"Lost {bet_amount} on {result}")
            return -bet_amount

def main():
    game = VirtualGame()
    print("🎉 Welcome to Virtual Color Prediction!")
    game.display_rules()

    while game.balance > 0:
        print(f"\n💰 Current Balance: {game.balance} coins")
        bet_details = game.place_bet()
        
        if not bet_details:
            continue
            
        color_choice, bet_amount = bet_details
        result = game.spin_wheel()
        outcome = game.update_balance(result, color_choice, bet_amount)
        
        print(f"\nResult: {result}")
        print(f"Outcome: {'+' if outcome >0 else ''}{outcome} coins")
        
        if game.balance <= 0:
            print("\n💸 Game Over - Virtual balance exhausted!")
            break
            
        if input("\nPlay again? (y/n): ").lower() != 'y':
            print(f"\n🏆 Final Balance: {game.balance} coins")
            break

    print("\n🔒 Remember: This is for entertainment only!")
    print("🚫 No real money value or prizes involved")

if __name__ == "__main__":
    main()
