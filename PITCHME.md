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
    t.index ["book_id"] # , name: 'my_name1', ...
    t.index ["shop_id", "book_id"], unique: true
  # ...
end
```

---

```bash
psql (9.6.2)
Type "help" for help.

my_db=# \d votes
                                      Table "public.votes"
    Column    |    Type |                     Modifiers
--------------+---------+----------------------------------------------------
 id           | integer | not null default nextval('votes_id_seq'::regclass)
 member_id    | integer | not null
 candidate_id | integer | not null
 value        | integer | not null default 0
 ndexes:
    "votes_pkey" PRIMARY KEY, btree (id)
    "index_votes_on_member_id_and_candidate_id" UNIQUE, btree (member_id, candidate_id)
    "index_votes_on_candidate_id" btree (candidate_id)
    "index_votes_on_member_id" btree (member_id)
Foreign-key constraints:
    "fk_rails_2bbb898ed4" FOREIGN KEY (candidate_id) REFERENCES candidates(id)
    "fk_rails_fdef739e66" FOREIGN KEY (member_id) REFERENCES members(id)
```
