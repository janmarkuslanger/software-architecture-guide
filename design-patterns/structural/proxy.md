# Proxy

The Proxy pattern provides a substitute object that controls access to another object. The proxy implements the same interface as the real object, so callers are unaware of the substitution. Common uses are lazy initialization, access control, logging, and caching.

## How it works

The proxy holds a reference to the real object (or creates it on demand) and forwards method calls to it. Before or after forwarding, the proxy can perform additional work: check permissions, log the call, return a cached result, or delay loading the real object until it is actually needed.

## Example

```python
from abc import ABC, abstractmethod


class ReportRepository(ABC):
    @abstractmethod
    def get_report(self, report_id: str) -> dict: ...


class DatabaseReportRepository(ReportRepository):
    def get_report(self, report_id: str) -> dict:
        print(f"[DB] Loading report {report_id} from database...")
        return {"id": report_id, "content": "Annual figures ..."}


class CachedReportProxy(ReportRepository):
    def __init__(self, repository: ReportRepository) -> None:
        self._repository = repository
        self._cache: dict[str, dict] = {}

    def get_report(self, report_id: str) -> dict:
        if report_id not in self._cache:
            self._cache[report_id] = self._repository.get_report(report_id)
        else:
            print(f"[Cache] Returning cached report {report_id}")
        return self._cache[report_id]


class AccessControlProxy(ReportRepository):
    def __init__(self, repository: ReportRepository, user_role: str) -> None:
        self._repository = repository
        self._user_role = user_role

    def get_report(self, report_id: str) -> dict:
        if self._user_role != "admin":
            raise PermissionError(f"Role '{self._user_role}' cannot access reports.")
        return self._repository.get_report(report_id)


db_repo = DatabaseReportRepository()
cached_repo = CachedReportProxy(db_repo)
secure_repo = AccessControlProxy(cached_repo, user_role="admin")

print(secure_repo.get_report("annual-2025"))
# [DB] Loading report annual-2025 from database...

print(secure_repo.get_report("annual-2025"))
# [Cache] Returning cached report annual-2025

guest_repo = AccessControlProxy(cached_repo, user_role="guest")
try:
    guest_repo.get_report("annual-2025")
except PermissionError as e:
    print(e)
# Role 'guest' cannot access reports.
```

## When to use

- You want to delay the creation of an expensive object until it is first needed (lazy initialization).
- You need to add access control around an object without modifying the object itself.
- You want to add cross-cutting concerns such as logging, caching, or retry logic to an object transparently.

## When not to use

- The overhead of the extra indirection and the added complexity outweigh the benefit. For simple objects, direct use is clearer.
- The proxy accumulates business logic. A proxy should control access or add cross-cutting concerns. It should not make domain decisions.
- You need the proxy behavior to vary significantly per call. In that case a more explicit design such as a middleware chain or decorator communicates the variation better.
