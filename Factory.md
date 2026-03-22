# 🧠 1. What is Factory Pattern?

👉 Instead of creating objects directly (`ClassA()`),
you **delegate object creation to a factory**.

### ❌ Without Factory

```python
model = LinearModel()
```

### ✅ With Factory

```python
model = ModelFactory.get_model("linear")
```

👉 You **decouple creation from usage**

---

# 🔥 2. Why Factory is Important (Real Backend Use)

* Select different **algorithms dynamically**
* Hide complex creation logic
* Improve scalability (add new types easily)

---

# 🧩 3. Types of Factory in Python

1. Simple Factory (most used)
2. Factory Method (OOP style)
3. Abstract Factory (advanced)

We’ll cover all with **distinct real-world examples** 👇

---

# 🚀 4. Example 1: ML Model Factory (Simple Factory ⭐⭐⭐)

👉 Real AI use case

```python
class LinearModel:
    def train(self):
        print("Training Linear Model")

class TreeModel:
    def train(self):
        print("Training Tree Model")


class ModelFactory:
    @staticmethod
    def get_model(model_type):
        if model_type == "linear":
            return LinearModel()
        elif model_type == "tree":
            return TreeModel()
        else:
            raise ValueError("Unknown model")


# Usage
model = ModelFactory.get_model("tree")
model.train()
```

---

# 🚀 5. Example 2: Payment System (Strategy + Factory combo 🔥)

👉 Backend system (very common in interviews)

```python
class UPI:
    def pay(self):
        print("Pay via UPI")

class Card:
    def pay(self):
        print("Pay via Card")


class PaymentFactory:
    def get_payment(self, method):
        if method == "upi":
            return UPI()
        elif method == "card":
            return Card()


# Usage
payment = PaymentFactory().get_payment("upi")
payment.pay()
```

---

# 🚀 6. Example 3: Notification Service (Extensible System)

👉 Add new channels without breaking code

```python
class Email:
    def send(self):
        print("Email sent")

class SMS:
    def send(self):
        print("SMS sent")


class NotificationFactory:
    @staticmethod
    def create(channel):
        mapping = {
            "email": Email,
            "sms": SMS
        }
        return mapping[channel]()


# Usage
notif = NotificationFactory.create("email")
notif.send()
```

---

# 🚀 7. Example 4: DB Connector Factory (Backend MUST KNOW ⭐⭐⭐)

```python
class MySQL:
    def connect(self):
        print("Connected to MySQL")

class MongoDB:
    def connect(self):
        print("Connected to MongoDB")


class DBFactory:
    @staticmethod
    def get_db(db_type):
        if db_type == "mysql":
            return MySQL()
        elif db_type == "mongo":
            return MongoDB()


# Usage
db = DBFactory.get_db("mongo")
db.connect()
```

👉 Used in:

* Microservices
* Config-based DB switching

---

# 🧱 8. Example 5: Factory Method Pattern (OOP Style)

👉 Instead of `if-else`, use subclasses

```python
from abc import ABC, abstractmethod

class Transport(ABC):
    @abstractmethod
    def deliver(self):
        pass


class Truck(Transport):
    def deliver(self):
        print("Deliver by road")


class Ship(Transport):
    def deliver(self):
        print("Deliver by sea")


class Logistics(ABC):
    @abstractmethod
    def create_transport(self):
        pass


class RoadLogistics(Logistics):
    def create_transport(self):
        return Truck()


class SeaLogistics(Logistics):
    def create_transport(self):
        return Ship()


# Usage
logistics = RoadLogistics()
transport = logistics.create_transport()
transport.deliver()
```

---

# 🏭 9. Abstract Factory (Advanced - System Design)

👉 Create **families of related objects**

```python
class Button:
    def render(self):
        pass

class WindowsButton(Button):
    def render(self):
        print("Windows Button")

class MacButton(Button):
    def render(self):
        print("Mac Button")


class GUIFactory:
    def create_button(self):
        pass


class WindowsFactory(GUIFactory):
    def create_button(self):
        return WindowsButton()


class MacFactory(GUIFactory):
    def create_button(self):
        return MacButton()


# Usage
factory = WindowsFactory()
btn = factory.create_button()
btn.render()
```

---

# ⚡ 10. Real Backend Mapping (VERY IMPORTANT)

| Use Case             | Pattern |
| -------------------- | ------- |
| ML model selection   | Factory |
| Payment gateway      | Factory |
| DB connections       | Factory |
| API versioning       | Factory |
| Notification service | Factory |

---

# ❌ Common Mistakes

* Overusing Factory for simple cases
* Putting too much logic inside factory
* Not using mapping/dictionary (Pythonic way)

---

# 🧠 Interview Answer (Golden Line)

👉 “Factory Pattern helps decouple object creation from usage, allowing dynamic selection of implementations and improving scalability.”

---
