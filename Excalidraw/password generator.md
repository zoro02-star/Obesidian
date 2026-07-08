```python 
import re
import secrets
import string


def check_strength(password):
    """Evaluates a password and returns a score out of 5."""
    score = 0
    if len(password) >= 12:
        score += 1
    if re.search(r"[a-z]", password):
        score += 1
    if re.search(r"[A-Z]", password):
        score += 1
    if re.search(r"\d", password):
        score += 1
    if re.search(r"[!@#$%^&*(),.?\":{}|<>]", password):
        score += 1

    levels = {0: "Weak", 1: "Weak", 2: "Fair", 3: "Good", 4: "Strong", 5: "Excellent"}
    return levels[score]


def generate_password(length=16):
    """Generates a cryptographically secure, high-strength password."""
    if length < 8:
        raise ValueError("Length must be at least 8 characters.")

    # Ensure password contains at least one of each character type
    categories = [string.ascii_lowercase, string.ascii_uppercase, string.digits, "!@#$%^&*()"]
    password = [secrets.choice(cat) for cat in categories]

    # Fill the remaining length with a mix of all characters
    all_chars = "".join(categories)
    password += [secrets.choice(all_chars) for _ in range(length - len(categories))]

    # Shuffle securely and join into a string
    secrets.SystemRandom().shuffle(password)
    return "".join(password)


# --- Main Execution ---
try:
    user_length = int(input("Enter desired password length (minimum 8): "))
    new_password = generate_password(user_length)

    print(f"\nGenerated Password: {new_password}")
    print(f"Password Strength Score: {check_strength(new_password)}")

except ValueError as e:
    print(f"Error: {e}. Please enter a valid integer.")

```