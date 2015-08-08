# Rendering
* `respond_to`:
  * `trips_controller.rb`, html
```
def show
  @trip = Trip.find(params[:id])
end
```
  * `trips_controller.rb`, html + json
```
def show
  @trip = Trip.find(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json {render json: @trip}
  end
end
```
  * `trips_controller.rb`, html with condition + json
```
def show
  @trip = Trip.find(params[:id])
  respond_to do |format|
    format.html do
      if @trip.promote?
        render :promote
      end
      # if not dead, reders normal show in html
    end
    format.json {render json: @trip}
  end
end
```
  * `trips_controller.rb`, html with condition only
```
def show
  @trip = Trip.find(params[:id])
  if @trip.promote?
    render :promote
  end
end
```
  * `trips_controller.rb`, json only
```
def show
  @trip = Trip.find(params[:id])
  render json: @trip
end
  * Only need `respond_to` when we need to repond to more then one thing.

* rendering status code list
```
:ok                   #200
:created              #201
:unprocessable_entity #422
:unauthorized         #401
:processing           #102
:not_found            #404
```
examplea
```
render json: @trip.errors, status: :unauthorized
render json: @trip, status: :created, location: @trip
```

