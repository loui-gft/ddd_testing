### Client requirements

Necesidades de datos de empresa:

    -Queremos saber qué trayectos están más hasta arriba de gente
    -Aplicación para gestionar sistema de rutas
    -Saber conductor y vehículo asignados a una ruta
    -Historial de conductor y vehículo
    -Consultar paradas de una ruta y modificarlas
    -Historial de incidencias del vehículo
    -Ver ubicación en tiempo real de los vehículos
    -Consultar y modificar horarios de una ruta
    -Que los conductores puedan introducir eventos
    -Según el tipo de evento, deberían enviarse notificaciones a los clientes
    -Poder asignar vehículos a una ruta
    -Poder asignar conductores a una ruta y un horario

Necesidades de aplicación para clientes:

    -Calculador de costo de viajes
    -Queremos que la gente busque horarios en una app
    -Ver notificaciones de retrasos





#### Model

```mermaid
---
title: Transport system entities
---
classDiagram
    class Route{
        <<Aggregate Root>>
        - String id
        - String name
        - List~Stop~ stops
        - List~Schedule~ schedules
        - List~VehicleHistory~ vehicleHistory
        - List~Passenger~ passengers
    }
    
    class Vehicle{
        <<Entity>>
        - String id
        - String plateNumber
        - Integer capacity
        - String status
        - String type
        - Coordinates location
        - List~Event~ events
        - Double speed
        - String direction
    }
    
    class Maintenance{
        <<Entity>>
        - String id
        - LocalDateTime date
        - String details
        - String status
    }
    
    class Passenger{
        <<Entity>>
        - String id
        - String name
        - String email
        - Integer phone
        - LocalDateTime registryDate
        - List~Trip~ trips
    }

    class Notification{
        <<Value Object>>
        - LocalDateTime timestamp
        - String Type
        - String title
        - String message
        - String severity
        - List~Action~ actions
    }
    
    class Stop{
        <<Entity>>
        -String id
        -String name
        -Coordinates location
    }
    
    class Driver{
        <<Entity>>
        - String id
        - String name
        - Integer contact
    }
    
    class Trip {
        <<Entity>>
        - String id
        - LocalDateTime startTime
        - LocalDateTime endTime
        - Stop startStop
        - Stop endStop
        - Double fare
    }  
    
    class Event{
        <<Value Object>>
        - String type
        - LocalDateTime time
        - String details
        - Maintenance maintenance
        - Notification notification
    }
    
    class Coordinates{
        <<Value Object>>
        - String latitude
        - String longitude
    }
    
    class Schedule {
        <<Value Object>>
        - String typeOfDay
        - LocalDateTime startTime
        - LocalDateTime endTime
        - Integer frequencyInMinutes
    }  
    
    class TypeOfDay {
        <<Enumeration>>
        DAYOFWEEK
        FRIDAY
        SATURDAY
        SUNDAY
        HOLIDAY
        EXCEPTION
    }  
    
    class Action {
        <<Value Object>>
        - String type
        - String status
    }

    class RouteHistory {
        <<Entity>>
        +String id
        +String vehicleId
        +String driverId
        +LocalDateTime startTime
        +LocalDateTime endTime
    }

    Route --> RouteHistory
    Route --> Passenger
    Route --> Stop
    Route --> Schedule
    Passenger --> Trip
    Trip --> Stop
    Vehicle --> Event
    Vehicle --> Coordinates
    RouteHistory --> Driver
    Event --> Notification
    Event --> Maintenance
    Schedule --> TypeOfDay
    RouteHistory --> Vehicle
    Stop --> Coordinates
    Notification --> Action

    class RouteFactory {
        <<Factory>>
        - createRoute(CreateRouteRequest createRouteRequest)
 }
 
    class CreateRouteRequest {
        <<Value Object>>
        - CreateRouteRequest(String name, List~Stop~ stops, List~Schedule~ schedules)
 }

    class RouteRepository {
        <<Repository>>
        - addRoute(Route route)
        - findRoute(String routeId)
        - findRoute(String name)
        - udpateRoute(Route route)
        - deleteRoute(String routeId)
    }
    
    class VehicleRepository {
        <<Repository>>
        - addVehicle(Vehicle vechile)
        - findVehicle(String vehicleId)
        - updateVehicle(Vehicle vehicle)
        - deleteVehicle(String vehicleId)
 }
    
 
    Route <--> RouteFactory
    Route <--> RouteRepository
    Vehicle <--> VehicleRepository
    RouteFactory <--> CreateRouteRequest



```