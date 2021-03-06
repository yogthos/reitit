# Name-based (reverse) Routing

All routes which have `:name` route data defined, can also be matched by name.

Given a router:

```clj
(require '[reitit.core :as r])

(def router
  (r/router
    ["/api"
     ["/ping" ::ping]
     ["/user/:id" ::user]]))
```

Listing all route names:

```clj
(r/route-names router)
; [:user/ping :user/user]
```

No match returns `nil`:

```clj
(r/match-by-name router ::kikka)
nil
```

Matching a route:

```clj
(r/match-by-name router ::ping)
; #Match{:template "/api/ping"
;        :data {:name :user/ping}
;        :result nil
;        :path-params {}
;        :path "/api/ping"}
```

If not all path-parameters are set, a `PartialMatch` is returned:

```clj
(r/match-by-name router ::user)
; #PartialMatch{:template "/api/user/:id",
;               :data {:name :user/user},
;               :result nil,
;               :path-params nil,
;               :required #{:id}}

(r/partial-match? (r/match-by-name router ::user))
; true
```

With provided path-parameters:

```clj
(r/match-by-name router ::user {:id "1"})
; #Match{:template "/api/user/:id"
;        :data {:name :user/user}
;        :path "/api/user/1"
;        :result nil
;        :path-params {:id "1"}}
```

There is also a exception throwing version:

```clj
(r/match-by-name! router ::user)
; ExceptionInfo missing path-params for route /api/user/:id: #{:id}
```
