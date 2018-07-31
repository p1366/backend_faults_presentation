# Backend General Faults

---

## Routing

---

#### Modifying data with @color[#DC143C](GET) request

```ruby
Rails.application.routes.draw do
  resource :sell_book, only: [:index]
  # ...
  get 'shops/sell'
end
```

---

#### Using @color[#DC143C](NON-STANDARD) actions

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

---

## Database
(blindfolded ActiveRecord)

---

#### FOREIGN KEY constraints
(absence)

```ruby
ActiveRecord::Schema.define(version: 2018_07_19_182936) do
  create_table "publishers", force: :cascade do |t|
  # ...
  create_table "books", force: :cascade do |t|
    t.bigint "publisher_id"
  # ...
  add_foreign_key "books", "publishers"
end
```

---

#### FOREIGN KEY constraints
(absence)

```ruby
class CreateBooks < ActiveRecord::Migration[5.1]
  def change
    create_table :books do |t|
      t.belongs_to :publisher, foreign_key: true
```

---

#### Index / UNIQUE Index
(absence)

```ruby
ActiveRecord::Schema.define(version: 2018_07_19_182936) do
  create_table "shop_books", force: :cascade do |t|
    t.bigint "book_id"
    t.bigint "shop_id"
    t.index ["book_id"], name: "my_index_1"
    t.index ["shop_id", "book_id"], name: "my_index_2", unique: true
  # ...
end
```
