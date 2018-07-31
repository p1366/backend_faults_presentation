# Backend General Faults

---

## Routing

---

### Modifying data with @color[#DC143C](GET) request

```ruby
Rails.application.routes.draw do
  resource :sell_book, only: [:index]
  # ...
  get 'shops/sell'
end
```

---

### Using @color[#DC143C](NON-STANDARD) actions

```ruby
Rails.application.routes.draw do
  resources :shops, only: :show do
    resources :books, only: :sale do
      member do
        put :sale
      end
  end
  # ...
  post '/shop/:id/sell' => 'shops#sell'
end
```
