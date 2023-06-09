# README

Wildlife Tracker Challenge
The Forest Service is considering a proposal to place in conservancy a forest of virgin Douglas fir just outside of Portland, Oregon. Before they give the go ahead, they need to do an environmental impact study. They've asked you to build an API the rangers can use to report wildlife sightings.

Story 1: In order to track wildlife sightings, as a user of the API, I need to manage animals.

✅ Branch: animal-crud-actions

Acceptance Criteria

✅ Create a resource for animal with the following information: common name and scientific binomial

```bash
$ rails g resource Animal common_name:string scientific_binomial:string
$ rails db:migrate
$ Animal.create(common_name: 'dog', scientific_binomial: 'canis lupus familiaris')
$ Animal.create(common_name: 'cat', scientific_binomial: 'felis catus')
$ Animal.create(common_name: 'black panther', scientific_binomial: 'panthera pardus')
```

✅ Can see the data response of all the animals

@ app/controllers/application_controller.rb
```rb
skip_before_action :verify_authenticity_token
```
@ app/controllers/animals_controller
```rb
class AnimalsController < ApplicationController

  def index
    animals = Animal.all
    render json:animals
  end

  def show
    animal = Animal.find(params[:id])
    render json:animal
  end

  def create
    animal = Animal.create(animal_params)
    if animal.valid?
      render json: animal
    else
      render json: animal.errors
    end
  end

  def update 
    animal =Animal.find(params[:id])
    animal.update(animal_params)
    if animal.valid?
      render json: animal
    else
      render json: animal.errors
    end
  end

  def destroy
    animal = Aminal.find(params[:id])
    if animal.destroy
      render json: animal
    else
      render json: animal.errors
    end
  end

  
  private
  
  def animal_params
    params.require(:animal).permit(:common_name, :scientific_binomial)
  end
end
```
@ Postman get request localhost:3000/animals
```json
[
  {
    "id": 1,
    "common_name": "dog",
    "scientific_binomial": "canis lupus familiaris",
    "created_at": "2023-04-21T18:34:43.230Z",
    "updated_at": "2023-04-21T18:34:43.230Z"
  },
  {
    "id": 2,
    "common_name": "cat",
    "scientific_binomial": "felis catus",
    "created_at": "2023-04-21T18:35:10.266Z",
    "updated_at": "2023-04-21T18:35:10.266Z"
  },
  {
    "id": 3,
    "common_name": "black panther",
    "scientific_binomial": "panthera pardus",
    "created_at": "2023-04-21T18:38:56.316Z",
    "updated_at": "2023-04-21T18:38:56.316Z"
  }
]
```
@ Postman get request localhost:3000/animals/3
```json
{
  "id": 3,
  "common_name": "black panther",
  "scientific_binomial": "panthera pardus",
  "created_at": "2023-04-21T18:38:56.316Z",
  "updated_at": "2023-04-21T18:38:56.316Z"
}
```

✅ Can create a new animal in the database

```json
{
    "common_name": "fox",
    "scientific_binomial": "vulpes vulpes"
}
```

✅ Can update an existing animal in the database

Can remove an animal entry in the database
Story 2: In order to track wildlife sightings, as a user of the API, I need to manage animal sightings.

Branch: sighting-crud-actions

Acceptance Criteria

Create a resource for animal sightings with the following information: latitude, longitude, date
Hint: An animal has_many sightings (rails g resource Sighting animal_id:integer ...)
Hint: Date is written in Active Record as yyyy-mm-dd (“2022-07-28")
Can create a new animal sighting in the database
Can update an existing animal sighting in the database
Can remove an animal sighting in the database
Story 3: In order to see the wildlife sightings, as a user of the API, I need to run reports on animal sightings.

Branch: animal-sightings-reports

Acceptance Criteria

Can see one animal with all its associated sightings
Hint: Checkout this example on how to include associated records
Can see all the all sightings during a given time period
Hint: Your controller can use a range to look like this:
class SightingsController < ApplicationController
  def index
    sightings = Sighting.where(date: params[:start_date]..params[:end_date])
    render json: sightings
  end
end
Hint: Be sure to add the start_date and end_date to what is permitted in your strong parameters method
Hint: Utilize the params section in Postman to ease the developer experience
Hint: Routes with params
Stretch Challenges
Story 4: In order to see the wildlife sightings contain valid data, as a user of the API, I need to include proper specs.

Branch: animal-sightings-specs

Acceptance Criteria
Validations will require specs in spec/models and the controller methods will require specs in spec/requests.

Can see validation errors if an animal doesn't include a common name and scientific binomial
Can see validation errors if a sighting doesn't include latitude, longitude, or a date
Can see a validation error if an animal's common name exactly matches the scientific binomial
Can see a validation error if the animal's common name and scientific binomial are not unique
Can see a status code of 422 when a post request can not be completed because of validation errors
Hint: Handling Errors in an API Application the Rails Way
Story 5: In order to increase efficiency, as a user of the API, I need to add an animal and a sighting at the same time.

Branch: submit-animal-with-sightings

Acceptance Criteria

Can create new animal along with sighting data in a single API request
Hint: Look into accepts_nested_attributes_for 