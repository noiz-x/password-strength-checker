# Password Strength Checker

An advanced Python tool for evaluating password strength based on entropy, common password wordlists, sequential and repeated patterns, and dictionary word checks. This utility provides actionable feedback to help users create stronger, more secure passwords. It is ideal for integration into web forms, DevSecOps pipelines, or as a standalone security tool.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Code Structure](#code-structure)
- [Customizing the Wordlist and Dictionary](#customizing-the-wordlist-and-dictionary)
- [Extending the Project](#extending-the-project)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Overview

The Password Strength Checker uses several strategies to determine how secure a given password is:

- **Entropy Calculation:** It calculates the base entropy using the size of the character set (lowercase, uppercase, digits, and special characters).
- **Pattern Penalties:** It applies penalties for repeated characters, sequential patterns (both ascending and descending), and common keyboard patterns.
- **Custom Wordlist Checks:** It verifies the password against a custom wordlist (e.g., `rockyou.txt`) for common passwords.
- **Dictionary Checks:** It optionally scans for dictionary words (or substrings) that may make the password easier to guess.
- **Feedback:** It provides detailed feedback and suggestions to help improve password security.

## Features

- **Dynamic Entropy Calculation:** Uses logarithmic calculations to estimate password strength.
- **Custom Wordlist Integration:** Easily load your own list of common passwords from a file.
- **Pattern Analysis:** Detects repeated characters and sequential patterns that reduce password strength.
- **Dictionary Word Detection:** Flags dictionary words or substrings that can compromise security.
- **Actionable Feedback:** Offers suggestions such as adding uppercase letters, numbers, or special characters.
- **Modular Design:** The code is organized into clear, reusable functions for easy integration and enhancement.

## Prerequisites

- Python 3.6 or later.
- A custom wordlist file (e.g., `rockyou.txt`) placed in the same directory as the script.
- Optionally, a dictionary file or set for additional word checks.

## Installation

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   ```

2. **Navigate to the project directory:**

   ```bash
   cd password_checker
   ```

3. **Ensure Python 3 is installed on your system.**

## Usage

Run the script directly from the command line:

```bash
python password_checker.py
```

Upon running, you will be prompted to enter a password. The tool will then output:

- The password's length.
- Its base entropy.
- Any penalties applied.
- The effective entropy.
- A qualitative strength rating (e.g., "Very Weak", "Strong").
- Detailed feedback and improvement suggestions.

## Code Structure

The project is organized into several functions:

- **`load_common_passwords(filepath: str) -> set`**  
  Loads your custom wordlist (e.g., `rockyou.txt`) into a Python set for fast lookup.

- **`calculate_base_entropy(password: str) -> float`**  
  Computes the base entropy of a password based on its length and the character set used.

- **`penalty_for_repeated_and_sequential_patterns(password: str) -> float`**  
  Detects repeated characters, sequential patterns, and common keyboard patterns (defined in `KEYBOARD_PATTERNS`), applying a penalty to the entropy.

- **`check_dictionary_words(password: str, dictionary: set) -> bool`**  
  Checks if the password or any substring (with a minimum length of 4 characters) exists in a provided dictionary.

- **`get_password_feedback(password: str, dictionary: set = None) -> (list, float)`**  
  Gathers feedback on password weaknesses, applying penalties and providing suggestions for improvement.

- **`evaluate_password_strength(password: str, dictionary: set = None) -> dict`**  
  Combines the entropy calculation and penalties to generate a final assessment of the password strength, along with detailed feedback.

## Customizing the Wordlist and Dictionary

### Loading a Custom Wordlist

Ensure your custom wordlist (e.g., `rockyou.txt`) is located in the same directory as the script. The file should contain one password per line. The code snippet below demonstrates how the file is loaded:

```python
import os

def load_common_passwords(filepath: str) -> set:
    try:
        with open(filepath, "r", encoding="utf-8") as file:
            return {line.strip().lower() for line in file if line.strip()}
    except FileNotFoundError:
        print(f"Error: Could not find the file: {filepath}")
        return set()

script_dir = os.path.dirname(os.path.abspath(__file__))
password_file = os.path.join(script_dir, "rockyou.txt")
COMMON_PASSWORDS = load_common_passwords(password_file)
```

### Using a Custom Dictionary

You can optionally pass a custom dictionary set for additional dictionary word checks. Update the `sample_dictionary` variable in the `__main__` block or modify the code to load the dictionary from a file.

## Extending the Project

- **Web Integration:** Embed the checker in web forms to provide real-time password feedback.
- **DevSecOps Pipelines:** Integrate as part of automated security checks in your development lifecycle.
- **Enhanced Feedback:** Expand the penalty and suggestion mechanisms, or integrate external libraries such as `zxcvbn` for even more advanced analysis.
- **GUI or API:** Build a graphical interface or RESTful API to make the tool accessible to a broader audience.

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Commit your changes with clear messages.
4. Submit a pull request for review.

Feel free to open issues for any bugs or feature requests.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Inspired by NIST password guidelines and industry best practices.
- Thanks to [Daniel Miessler](https://github.com/danielmiessler) for the `rockyou.txt` wordlist, which has been invaluable for password analysis.
- Special thanks to developers who have contributed to similar projects and shared their insights online.
