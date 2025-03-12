# Homework on "Flow Control Algorithms and Rate Limiting"

## Welcome 🌞

Today, you will practice applying data flow control algorithms to solve
practical tasks.

Imagine that in a chat system, you need to implement a mechanism to limit the
frequency of messages sent by users to prevent spam. You must do this in two
ways:

1. **Using the Sliding Window Algorithm** to precisely control time intervals.
   This allows tracking the number of messages within a given time window and
   restricting users from sending messages if the limit is exceeded.
2. **Using the Throttling Algorithm** to control the time intervals between
   messages. This ensures a fixed waiting interval between user messages and
   limits the sending frequency if this interval is not respected.

Are you ready? Let's get to work!

Good luck! 😎

---

## Task 1: Implementing a Rate Limiter Using the Sliding Window Algorithm

In a chat system, you need to implement a mechanism to limit the frequency of
messages sent by users to prevent spam. The implementation must use the
**Sliding Window Algorithm** for precise time interval control, which allows
tracking the number of messages within a given time window and restricting users
if the limit is exceeded.

### **Technical Requirements**

- The implementation must use the **Sliding Window Algorithm** for accurate time
  interval control.
- **Basic system parameters**:

  - `window_size` — 10 seconds
  - `max_requests` — 1 message per window

- Implement the `SlidingWindowRateLimiter` class.

- Implement the following class methods:

  - `_cleanup_window` — removes outdated requests from the window and updates
    the active time window.
  - `can_send_message` — checks whether sending a message is allowed within the
    current time window.
  - `record_message` — records a new message and updates the user's history.
  - `time_until_next_allowed` — calculates the wait time until the next message
    can be sent.

- **Data structure for storing message history**: `collections.deque`.

---

### **Acceptance Criteria**

📌 **These criteria must be met for the homework to be reviewed by a mentor.**  
If any criterion is not met, the mentor will return the assignment for revision
without evaluation.  
If you "just need clarification" 😉 or are stuck on a specific step, reach out
to your mentor on Slack.

- Attempting to send a message **earlier than 10 seconds** returns `False` from
  the `can_send_message` method.
- The **first message** from a user always returns `True`.
- When all messages are removed from the user's window, the user record is
  deleted from the data structure.
- The `time_until_next_allowed` method returns the wait time in seconds.
- The test function runs successfully and behaves as expected based on the
  provided example.

---

## **Task Template**

```python
import random from typing import Dict import time from collections import deque

class SlidingWindowRateLimiter: def **init**(self, window_size: int = 10,
max_requests: int = 1): pass def \_cleanup_window(self, user_id: str,
current_time: float) -> None: pass

    def can_send_message(self, user_id: str) -> bool:
        pass

    def record_message(self, user_id: str) -> bool:
        pass

    def time_until_next_allowed(self, user_id: str) -> float:
        pass

# Демонстрація роботи

def test_rate_limiter(): # Створюємо rate limiter: вікно 10 секунд, 1
повідомлення limiter = SlidingWindowRateLimiter(window_size=10, max_requests=1)

    # Симулюємо потік повідомлень від користувачів (послідовні ID від 1 до 20)
    print("\\n=== Симуляція потоку повідомлень ===")
    for message_id in range(1, 11):
        # Симулюємо різних користувачів (ID від 1 до 5)
        user_id = message_id % 5 + 1

        result = limiter.record_message(str(user_id))
        wait_time = limiter.time_until_next_allowed(str(user_id))

        print(f"Повідомлення {message_id:2d} | Користувач {user_id} | "
              f"{'✓' if result else f'× (очікування {wait_time:.1f}с)'}")

        # Невелика затримка між повідомленнями для реалістичності
        # Випадкова затримка від 0.1 до 1 секунди
        time.sleep(random.uniform(0.1, 1.0))

    # Чекаємо, поки вікно очиститься
    print("\\nОчікуємо 4 секунди...")
    time.sleep(4)

    print("\\n=== Нова серія повідомлень після очікування ===")
    for message_id in range(11, 21):
        user_id = message_id % 5 + 1
        result = limiter.record_message(str(user_id))
        wait_time = limiter.time_until_next_allowed(str(user_id))
        print(f"Повідомлення {message_id:2d} | Користувач {user_id} | "
              f"{'✓' if result else f'× (очікування {wait_time:.1f}с)'}")
        # Випадкова затримка від 0.1 до 1 секунди
        time.sleep(random.uniform(0.1, 1.0))

if **name** == "**main**": test_rate_limiter()
```

# **Expected Output**

```python
=== Симуляція потоку повідомлень === Повідомлення 1 | Користувач 2 | ✓
Повідомлення 2 | Користувач 3 | ✓ Повідомлення 3 | Користувач 4 | ✓ Повідомлення
4 | Користувач 5 | ✓ Повідомлення 5 | Користувач 1 | ✓ Повідомлення 6 |
Користувач 2 | × (очікування 7.0с) Повідомлення 7 | Користувач 3 | × (очікування
6.5с) Повідомлення 8 | Користувач 4 | × (очікування 7.0с) Повідомлення 9 |
Користувач 5 | × (очікування 6.8с) Повідомлення 10 | Користувач 1 | ×
(очікування 7.4с)

Очікуємо 4 секунди...

=== Нова серія повідомлень після очікування === Повідомлення 11 | Користувач 2 |
× (очікування 1.0с) Повідомлення 12 | Користувач 3 | × (очікування 0.7с)
Повідомлення 13 | Користувач 4 | × (очікування 0.4с) Повідомлення 14 |
Користувач 5 | × (очікування 0.0с) Повідомлення 15 | Користувач 1 | ✓
Повідомлення 16 | Користувач 2 | ✓ Повідомлення 17 | Користувач 3 | ✓
Повідомлення 18 | Користувач 4 | ✓ Повідомлення 19 | Користувач 5 | ✓
Повідомлення 20 | Користувач 1 | × (очікування 7.0с)
```

---

## **Task 2: Implementing a Rate Limiter Using the Throttling Algorithm**

In a chat system, you need to implement a mechanism to limit the frequency of
messages sent by users to prevent spam. The implementation must use the
**Throttling Algorithm** to control the time intervals between messages,
ensuring a fixed waiting period between user messages and restricting the
sending frequency if this interval is not maintained.

### **Technical Requirements**

- The implementation must use the **Throttling Algorithm** to control time
  intervals.
- **Basic system parameter**:
  - `min_interval` — 10 seconds (minimum interval between messages).
- Implement the `ThrottlingRateLimiter` class.
- Implement the following class methods:
  - `can_send_message` — checks whether sending a message is allowed based on
    the time of the last message.
  - `record_message` — records a new message and updates the timestamp of the
    last message.
  - `time_until_next_allowed` — calculates the time until the next message can
    be sent.
- **Data structure for storing the last message timestamp**: `Dict[str, float]`.

---

## **Task Template**

```python
import time from typing import Dict import random

class ThrottlingRateLimiter: def **init**(self, min_interval: float = 10.0):
pass

    def can_send_message(self, user_id: str) -> bool:
        pass

    def record_message(self, user_id: str) -> bool:
        pass

    def time_until_next_allowed(self, user_id: str) -> float:
        pass

def test_throttling_limiter(): limiter =
ThrottlingRateLimiter(min_interval=10.0)

    print("\\n=== Симуляція потоку повідомлень (Throttling) ===")
    for message_id in range(1, 11):
        user_id = message_id % 5 + 1

        result = limiter.record_message(str(user_id))
        wait_time = limiter.time_until_next_allowed(str(user_id))

        print(f"Повідомлення {message_id:2d} | Користувач {user_id} | "
              f"{'✓' if result else f'× (очікування {wait_time:.1f}с)'}")

        # Випадкова затримка між повідомленнями
        time.sleep(random.uniform(0.1, 1.0))

    print("\\nОчікуємо 10 секунд...")
    time.sleep(10)

    print("\\n=== Нова серія повідомлень після очікування ===")
    for message_id in range(11, 21):
        user_id = message_id % 5 + 1
        result = limiter.record_message(str(user_id))
        wait_time = limiter.time_until_next_allowed(str(user_id))
        print(f"Повідомлення {message_id:2d} | Користувач {user_id} | "
              f"{'✓' if result else f'× (очікування {wait_time:.1f}с)'}")
        time.sleep(random.uniform(0.1, 1.0))

if **name** == "**main**": test_throttling_limiter()
```

# **Expected Output**

```python
=== Симуляція потоку повідомлень (Throttling) === Повідомлення 1 | Користувач 2
| ✓ Повідомлення 2 | Користувач 3 | ✓ Повідомлення 3 | Користувач 4 | ✓
Повідомлення 4 | Користувач 5 | ✓ Повідомлення 5 | Користувач 1 | ✓ Повідомлення
6 | Користувач 2 | × (очікування 7.4с) Повідомлення 7 | Користувач 3 | ×
(очікування 7.6с) Повідомлення 8 | Користувач 4 | × (очікування 7.6с)
Повідомлення 9 | Користувач 5 | × (очікування 7.6с) Повідомлення 10 | Користувач
1 | × (очікування 7.4с)

Очікуємо 4 секунди...

=== Нова серія повідомлень після очікування === Повідомлення 11 | Користувач 2 |
× (очікування 0.7с) Повідомлення 12 | Користувач 3 | × (очікування 0.6с)
Повідомлення 13 | Користувач 4 | × (очікування 0.5с) Повідомлення 14 |
Користувач 5 | ✓ Повідомлення 15 | Користувач 1 | ✓ Повідомлення 16 | Користувач
2 | ✓ Повідомлення 17 | Користувач 3 | ✓ Повідомлення 18 | Користувач 4 | ✓
Повідомлення 19 | Користувач 5 | × (очікування 7.9с) Повідомлення 20 |
Користувач 1 | × (очікування 7.7с)
```
