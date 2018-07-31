General Faults
===

---

## Routing

---

### Modifying data with GET request

```ruby
Rails.application.routes.draw do
  resource :sell_book, only: [:index]
  # ...
  get 'shops/sell'
end
```
