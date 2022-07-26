# README 

Cree un nuevo proyecto llamado 'dojo_ninjas' y haga que utilice la base de datos SQLite predeterminada para el entorno de desarrollo y pruebas. Para este proyecto, vas a necesitar las siguientes tablas/modelos. Por favor vea el siguiente diagrama y cree los modelos necesarios.
### Empieza un nuevo proyecto (el nombre del proyecto debe ser 'dojo_ninjas').
rails new dojo_ninjas
### Crea las tablas/modelos apropiados.
rails g model dojo name:string city:string state:string
rails db:migrate
rails g model ninja first_name:string last_name:string dojo:reference
rails db:migrate
## Usando la consola de Ruby
### Cree 3 dojos (ingrese algunos datos en blanco solo para asegurarse que le permite ingresar datos en blanco).
Dojo.create(name:"", city:"Mountain View", state:"CA")
Dojo.create(name:"CodingDojo Seattle", city:"", state:"WA")
Dojo.create(name:"CodingDojo New York", city:"New York", state:"NY")
### Elimine los 3 dojos que creó (ej. Dojo.find(1).destroy, también consulte el método destroy_all).
Dojo.find(1).destroy
Dojo.destroy_all
Cambia tu modelo para que haga las siguientes validaciones:
### Para dojo, requerir nombre, ciudad, estado. También haga que el estados sea de 2 caracteres de longitud. 
```
 class Dojo < ApplicationRecord
  has_many :ninjas
  validates :name, presence: true
  validates :city, presence: true
  validates :state, presence: true, lengh: { is:2 }
 end
```
# Asegúrate que el modelo ninja pertenece a dojo (especifique esto en ambos modelos tanto dojo como en ninja).
# Para ninja, requerir nombre y apellido.
```
class Ninja < ApplicationRecord
    belongs_to :dojo
    validates :first_name, presence: true
    validates :last_name, presence: true
end
```
## Creacion de dojos
# Dojo
 * Haga que incluya el nombre del dojo, la ciudad y el estado de cada dojo.
 * Haga que el primero dojo sea "CodingDojo Silicon Valley" en "Mountain View", "CA".
 * Haga que el segundo dojo sea "CodingDojo Seattle" en "Seattle", "WA".
 * Haga que el tercer dojo sea "CodingDojo New York" en "New York", "NY".
Dojo.create(name:"CodingDojo Silicon Valley", city:"Mountain View", state:"CA")
Dojo.create(name:"CodingDojo Seattle", city:"Seattle", state:"WA")
Dojo.create(name:"CodingDojo New York", city:"New York", state:"NY")
## Creacion de Ninja
# Ninja
 * Haga que incluya el nombre y apellido de cada ninja en el dojo.
 * Cada dojo puede tener múltiples ninjas y cada ninja pertenece a un dojo específico.
 * Cree 3 ninjas que pertenezcan al primer dojo que creó.
Ninja.create(first_name:"Juan", last_name:"Perez",dojo_id:1)
Ninja.create(first_name:"Maria", last_name:"Lopez",dojo_id:1)
Ninja.create(first_name:"Manuel", last_name:"Hernandez",dojo_id:1)
# Cree 3 ninjas más que pertenezcan al segundo dojo que creó.
Ninja.create(first_name:"Felipe", last_name:"Ping",dojo_id:2)
Ninja.create(first_name:"Francisco", last_name:"Matinez",dojo_id:2)
Ninja.create(first_name:"Josefa", last_name:"Sandoval",dojo_id:2)
# Cree 3 ninjas más que pertenezcan al tercer dojo que creó.
Ninja.create(first_name:"Jaime", last_name:"Vidal",dojo_id:3)
Ninja.create(first_name:"Matias", last_name:"Abarca",dojo_id:3)
Ninja.create(first_name:"Carolina", last_name:"Perez",dojo_id:3)
# Cree 3 dojos adicionales usando el comando Dojo.new.
dojo = Dojo.new(name:"CodingDojo Chile", city:"Santiago", state:"ST") 
dojo.save
# Intente crear algunos dojos adicionales pero sin especificar algunos de los campos requeridos. Asegúrese que las validaciones están funcionando y que le devuelve los mensajes de error.
```
Dojo.create!(name:"", city:"", state:"")
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/activerecord-6.1.6/lib/active_record/validations.rb:80:in `raise_validation_error': Validation failed: Name can't be blank, City can't be blank, State can't be blank, State is the wrong length (should be 2 characters) (ActiveRecord::RecordInvalid)
```
# Asegúrate de entender cómo obtener todos los ninjas para cualquier dojo (ej. obtener todos los ninjas para el primer dojo, segundo dojo, etc).
```
Dojo.find(1).ninjas
Dojo Load (1.0ms)  SELECT "dojos".* FROM "dojos" WHERE "dojos"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
  Ninja Load (0.7ms)  SELECT "ninjas".* FROM "ninjas" WHERE "ninjas"."dojo_id" = ?  [["dojo_id", 1]]
=>
[#<Ninja:0x000002a7bcb77968
  id: 1,
  first_name: "Juan",
  last_name: "Perez",
  dojo_id: 1,
  created_at: Tue, 26 Jul 2022 01:20:21.096877000 UTC +00:00,
  updated_at: Tue, 26 Jul 2022 01:20:21.096877000 UTC +00:00>,
 #<Ninja:0x000002a7bcb774e0
  id: 2,
  first_name: "Maria",
  last_name: "Lopez",
  dojo_id: 1,
  created_at: Tue, 26 Jul 2022 01:22:57.539375000 UTC +00:00,
  updated_at: Tue, 26 Jul 2022 01:22:57.539375000 UTC +00:00>,
 #<Ninja:0x000002a7bcb77378
  id: 3,
  first_name: "Manuel",
  last_name: "Hernandez",
  dojo_id: 1,
  created_at: Tue, 26 Jul 2022 01:23:24.129100000 UTC +00:00,
  updated_at: Tue, 26 Jul 2022 01:23:24.129100000 UTC +00:00>]
  ```
 # ¿Cómo recuperar solo el nombre de los ninjas que pertenecen al segundo dojo y ordenar el resultado por created_at en orden descendiente?
   Dojo.find(2).ninjas.order('created_at DESC')
 # Elimine el segundo dojo. ¿Cómo podrías ajustar tu modelo para que automáticamente elimine todos los ninjas asociados con ese dojo?
 class Dojo < ApplicationRecord
   ## before_destroy { ninjas.destroy_all }
   ```
    has_many :ninjas
    validates :name, presence: true
    validates :city, presence: true
    validates :state, presence: true, length: { is:2 }
end
```
```
-> Dojo.find(2).destroy
irb(main):003:0> Dojo.find(2).destroy
  Dojo Load (0.7ms)  SELECT "dojos".* FROM "dojos" WHERE "dojos"."id" = ? LIMIT ?  [["id", 2], ["LIMIT", 1]]
  TRANSACTION (0.7ms)  begin transaction
  Ninja Load (0.4ms)  SELECT "ninjas".* FROM "ninjas" WHERE "ninjas"."dojo_id" = ?  [["dojo_id", 2]]
  Ninja Destroy (10.1ms)  DELETE FROM "ninjas" WHERE "ninjas"."id" = ?  [["id", 4]]
  Ninja Destroy (0.7ms)  DELETE FROM "ninjas" WHERE "ninjas"."id" = ?  [["id", 5]]
  Ninja Destroy (0.8ms)  DELETE FROM "ninjas" WHERE "ninjas"."id" = ?  [["id", 6]]
  Dojo Destroy (0.6ms)  DELETE FROM "dojos" WHERE "dojos"."id" = ?  [["id", 2]]
  TRANSACTION (20.8ms)  commit transaction
=>
#<Dojo:0x000002759796c990
 id: 2,
 name: "CodingDojo Seattle",
 city: "Seattle",
 state: "WA",
 created_at: Tue, 26 Jul 2022 01:20:01.985555000 UTC +00:00,
 updated_at: Tue, 26 Jul 2022 01:20:01.985555000 UTC +00:00>
...