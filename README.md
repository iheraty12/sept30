# sept30

import tkinter as tk
from tkinter import messagebox

class GuessNumberGame:
    def __init__(self, root):
        self.root = root
        self.root.title("Guess the Number Game")

        # Initialize variables
        self.low = 1
        self.high = 100
        self.computer_guess = None
        self.attempts = 0
        self.game_over = False

        # Create widgets
        self.guess_label = tk.Label(root, text="I am thinking of a number between 1 and 100...")
        self.guess_label.pack(pady=10)

        self.result_label = tk.Label(root, text="")
        self.result_label.pack(pady=10)

        self.buttons_frame = tk.Frame(root)
        self.buttons_frame.pack(pady=10)

        self.too_small_button = tk.Button(self.buttons_frame, text="Too small", command=self.too_small)
        self.too_small_button.grid(row=0, column=0, padx=5)

        self.too_large_button = tk.Button(self.buttons_frame, text="Too large", command=self.too_large)
        self.too_large_button.grid(row=0, column=1, padx=5)

        self.correct_button = tk.Button(self.buttons_frame, text="Correct", command=self.correct)
        self.correct_button.grid(row=0, column=2, padx=5)

        self.new_game_button = tk.Button(root, text="New game", command=self.new_game)
        self.new_game_button.pack(pady=10)

        # Start the first game
        self.new_game()

    def new_game(self):
        """Start a new game by resetting values."""
        self.low = 1
        self.high = 100
        self.attempts = 0
        self.game_over = False
        self.result_label.config(text="")
        self.guess_label.config(text="I am thinking of a number between 1 and 100...")
        self.too_small_button.config(state=tk.NORMAL)
        self.too_large_button.config(state=tk.NORMAL)
        self.correct_button.config(state=tk.NORMAL)
        self.make_guess()

    def make_guess(self):
        """Make the computer guess and display it."""
        if not self.game_over:
            self.computer_guess = (self.low + self.high) // 2
            self.guess_label.config(text=f"My guess is: {self.computer_guess}")
            self.attempts += 1

    def too_small(self):
        """Handle the user's hint that the guess was too small."""
        if self.computer_guess is not None and not self.game_over:
            self.low = self.computer_guess + 1
            self.make_guess()

    def too_large(self):
        """Handle the user's hint that the guess was too large."""
        if self.computer_guess is not None and not self.game_over:
            self.high = self.computer_guess - 1
            self.make_guess()

    def correct(self):
        """Handle the correct guess and end the game."""
        if self.computer_guess is not None and not self.game_over:
            self.game_over = True
            self.result_label.config(text=f"I guessed it in {self.attempts} attempts!")
            self.too_small_button.config(state=tk.DISABLED)
            self.too_large_button.config(state=tk.DISABLED)
            self.correct_button.config(state=tk.DISABLED)
            messagebox.showinfo("Game Over", f"I guessed your number {self.computer_guess} in {self.attempts} attempts!")

if __name__ == "__main__":
    root = tk.Tk()
    game = GuessNumberGame(root)
    root.mainloop()
