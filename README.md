import random
import time

# Tennis Game - Console/Online Version
class TennisGame:
    def __init__(self):
        self.score_player = 0
        self.score_ai = 0
        self.ball_position = 50
        self.paddle_player = 50
        self.paddle_ai = 50
        self.ball_direction = 1
        self.game_running = True
    
    def display_court(self):
        """Display the game court"""
        print("\n" * 2)
        print("=" * 60)
        print("TENNIS GAME - Python Edition")
        print("=" * 60)
        
        # Draw court
        for row in range(25):
            line = ""
            for col in range(50):
                # Paddles
                if col == 0 and abs(row - self.paddle_player) <= 2:
                    line += "█"
                elif col == 49 and abs(row - self.paddle_ai) <= 2:
                    line += "█"
                # Ball
                elif col == self.ball_position and row == 12:
                    line += "●"
                # Court lines
                elif col == 25 and row % 2 == 0:
                    line += "|"
                else:
                    line += "·"
            print(line)
        
        print("=" * 60)
        print(f"PLAYER: {self.score_player}  |  AI: {self.score_ai}")
        print("=" * 60)
    
    def get_player_input(self):
        """Get player movement input"""
        print("\nEnter move (W=up, S=down, or just press Enter):")
        move = input().upper()
        
        if move == "W" and self.paddle_player > 2:
            self.paddle_player -= 3
        elif move == "S" and self.paddle_player < 20:
            self.paddle_player += 3
    
    def ai_move(self):
        """AI paddle movement"""
        if self.paddle_ai < self.ball_position - 2 and self.paddle_ai < 20:
            self.paddle_ai += 2
        elif self.paddle_ai > self.ball_position + 2 and self.paddle_ai > 2:
            self.paddle_ai -= 2
    
    def update_ball(self):
        """Update ball position"""
        self.ball_position += self.ball_direction
        
        # Ball hits top or bottom wall
        if self.ball_position <= 0 or self.ball_position >= 49:
            self.ball_direction *= -1
        
        # Ball hits player paddle
        if self.ball_position <= 2 and abs(self.ball_position - self.paddle_player) <= 3:
            self.ball_direction = 1
            self.score_player += 1
        
        # Ball hits AI paddle
        elif self.ball_position >= 47 and abs(self.ball_position - self.paddle_ai) <= 3:
            self.ball_direction = -1
            self.score_ai += 1
        
        # Ball goes out (point scored)
        elif self.ball_position < 0:
            self.score_ai += 1
            self.reset_ball()
        elif self.ball_position > 50:
            self.score_player += 1
            self.reset_ball()
    
    def reset_ball(self):
        """Reset ball to center"""
        self.ball_position = 25
        self.ball_direction = random.choice([-1, 1])
    
    def play(self):
        """Main game loop"""
        print("Welcome to Tennis Game!")
        print("Controls: W (up), S (down)")
        print("First to 5 points wins!\n")
        
        while self.game_running:
            self.display_court()
            
            # Check win condition
            if self.score_player >= 5:
                print("\n🎉 YOU WIN! 🎉")
                self.game_running = False
                break
            elif self.score_ai >= 5:
                print("\n😢 AI WINS! 😢")
                self.game_running = False
                break
            
            # Get input
            self.get_player_input()
            
            # AI move
            self.ai_move()
            
            # Update ball
            self.update_ball()
            
            # Slow down for visibility
            time.sleep(0.5)

# Run the game
if __name__ == "__main__":
    game = TennisGame()
    game.play()
