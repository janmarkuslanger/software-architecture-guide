# Singleton

The Singleton pattern ensures that a class has exactly one instance and provides a global point of access to it. No matter how often you ask for the instance, you always get the same object back.

## How it works

The constructor is made private (or protected) so nothing outside the class can create a new instance directly. Note: Not every language like Javascript supports private methods. The class itself holds the single instance as a class-level variable and exposes it through a static method or property. On the first call the instance is created; on every subsequent call the existing one is returned.

## Example

```python
class AppConfig:
    _instance: "AppConfig | None" = None

    def __new__(cls) -> "AppConfig":
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.settings = {}
        return cls._instance

    def set(self, key: str, value: str) -> None:
        self.settings[key] = value

    def get(self, key: str) -> str | None:
        return self.settings.get(key)


config_a = AppConfig()
config_a.set("theme", "dark")

config_b = AppConfig()
print(config_b.get("theme"))  # dark
print(config_a is config_b)   # True
```

## When to use

- You need exactly one shared instance of something across the whole application, such as a configuration registry, a logger, or a connection pool.
- Creating the object is expensive and you want to do it only once.
- Global state is genuinely needed and you want to make that dependency explicit rather than hidden.

## When not to use

- The class holds mutable state that different parts of the application need to see independently. This shared mutable state leads to hard-to-trace bugs.
- You are writing code that needs to be tested in isolation. Singletons survive between test runs and cause test pollution.
- You are reaching for Singleton only because you want a convenient global variable. That is a code smell, not a design decision.
