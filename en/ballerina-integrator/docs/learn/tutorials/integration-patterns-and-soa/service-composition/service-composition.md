---
title: Service Composition
commitHash: a76e9f433c9db062427fd53e641e158cc9223de9
note: This is an auto-generated file do not edit this, You can edit content in "ballerina-integrator" repo
---

A service composition is an aggregate of services collectively composed to automate a particular task or business process. 

> This guide walks you through the process of implementing a service composition using Ballerina language. 

The following are the sections available in this guide.

- [What you'll build](#what-youll-build)
- [Prerequisites](#prerequisites)
- [Implementation](#implementation)
- [Testing](#testing)
- [Deployment](#deployment)

## What you’ll build
To understand how you can build a service composition using Ballerina, let's consider a real-world use case of a Travel agency that arranges complete tours for users. A tour package includes airline ticket reservation and hotel room reservation. Therefore, the Travel agency service requires communicating with other necessary back-ends. The following diagram illustrates this use case clearly.

![alt text](../../../../../assets/img/service-composition.svg)

Travel agency is the service that acts as the composition initiator. The other three services are external services that the travel agency service calls to do airline ticket booking and hotel reservation. These are not necessarily Ballerina services and can theoretically be third-party services that the travel agency service calls to get things done. However, for the purposes of setting up this scenario and illustrating it in this guide, these third-party services are also written in Ballerina.

## Prerequisites
- [Ballerina Distribution](https://ballerina.io/learn/getting-started/)
- A Text Editor or an IDE 
> **Tip**: For a better development experience, install one of the following Ballerina IDE plugins: [VSCode](https://marketplace.visualstudio.com/items?itemName=ballerina.ballerina), [IntelliJ IDEA](https://plugins.jetbrains.com/plugin/9520-ballerina)

## Implementation

> If you want to skip the basics, you can download the git repo and directly move to the `Testing` section by skipping `Implementation` section.

### Create the project structure

1. Create a new project `service-composition`.

    ```bash
    $ ballerina new  service-composition
    ```

2. Create a module named `guide` inside the project directory.

    ```bash
    $ ballerina add guide
    ```

Use the following module structure for this guide.

```
service-composition
├── Ballerina.toml
└── src
   └── guide
      ├── airline_reservation_service.bal
      ├── hotel_reservation_service.bal
      ├── travel_agency_service.bal
      ├── Module.md
      └── tests
          └── travel_agency_service_test.bal
```

Create the above directories in your local machine and copy the below given `.bal` files to appropriate locations.

### Developing the service

Let's look at the implementation of the travel agency service, which acts as the composition initiator.

Arranging a complete tour travel agency service requires communicating with three other services: airline reservation and hotel reservation. All these services accept POST requests with appropriate JSON payloads and send responses back with JSON payloads. Request and response payloads are similar for all three backend services.

Sample request payload:
```bash
{"Name":"Bob", "ArrivalDate":"12-03-2020", "DepartureDate":"13-04-2022", 
 "Preference":<service_dependent_preference>};
```

Sample response payload:

```bash
{"Status":"Success"}
```

When a client initiates a request to arrange a tour, the travel agency service first needs to communicate with the airline reservation service to book a flight ticket. To check the implementation of airline reservation service, see the `airline_reservation_service.bal` implementation.

Once the airline ticket reservation is successful, the travel agency service needs to communicate with the hotel reservation service to reserve hotel rooms. To check the implementation of hotel reservation service, see the `hotel_reservation_service.bal` implementation.

If all services work successfully, the travel agency service confirms and arranges the complete tour for the user. Refer to the `travel_agency_service.bal` to see the complete implementation of the travel agency service. Inline comments are added for better understanding.


**travel_agency_service.bal**
```ballerina
import ballerina/http;

// Service endpoint
listener http:Listener travelAgencyEP = new(9090);

// Client endpoint to communicate with Airline reservation service
http:Client airlineReservationEP = new("http://localhost:9091/airline");

// Client endpoint to communicate with Hotel reservation service
http:Client hotelReservationEP = new("http://localhost:9092/hotel");

// Travel agency service to arrange a complete tour for a user
@http:ServiceConfig {basePath:"/travel"}
service travelAgencyService on travelAgencyEP {

    // Resource to arrange a tour
    @http:ResourceConfig {methods:["POST"], consumes:["application/json"], produces:["application/json"]}
    resource function arrangeTour(http:Caller caller, http:Request inRequest) returns error? {
        http:Response outResponse = new;
        json inReqPayload = {};

        // CODE-SEGMENT-BEGIN: segment_1
        // Try parsing the JSON payload from the user request
        var payload = inRequest.getJsonPayload();
        if (payload is json) {
            // Valid JSON payload
            inReqPayload = payload;
        } else {
            // NOT a valid JSON payload
            outResponse.statusCode = 400;
            outResponse.setJsonPayload({"Message":"Invalid payload - Not a valid JSON payload"});
            var result = caller->respond(outResponse);
            handleError(result);
            return;
        }

        // Json payload format for an http out request
        json outReqPayload = {
            Name: check inReqPayload.Name,
            ArrivalDate: check inReqPayload.ArrivalDate,
            DepartureDate: check inReqPayload.DepartureDate,
            Preference:""
        };

        json airlinePreference = check inReqPayload.Preference.Airline;
        json hotelPreference = check inReqPayload.Preference.Accommodation;
        json carPreference = check inReqPayload.Preference.Car;

        // If payload parsing fails, send a "Bad Request" message as the response
        if (outReqPayload.Name == () || outReqPayload.ArrivalDate == () || outReqPayload.DepartureDate == () ||
            airlinePreference == () || hotelPreference == () || carPreference == ()) {
            outResponse.statusCode = 400;
            outResponse.setJsonPayload({"Message":"Bad Request - Invalid Payload"});
            var result = caller->respond(outResponse);
            handleError(result);
            return;
        }
        // CODE-SEGMENT-END: segment_1

        // CODE-SEGMENT-BEGIN: segment_2
        // Reserve airline ticket for the user by calling Airline reservation service
        // construct the payload
        json outReqJsonPayloadAirline = {
            Name: check outReqPayload.Name,
            ArrivalDate: check outReqPayload.ArrivalDate,
            DepartureDate: check outReqPayload.DepartureDate,
            Preference: airlinePreference
        };

        http:Request outReqPayloadAirline = new;
        outReqPayloadAirline.setJsonPayload(<@untainted>outReqJsonPayloadAirline);

        // Send a post request to airlineReservationService with appropriate payload and get response
        http:Response inResAirline = check airlineReservationEP->post("/reserve", outReqPayloadAirline);

        // Get the reservation status
        var airlineResPayload = check inResAirline.getJsonPayload();
        string airlineStatus = airlineResPayload.Status.toString();
        // If reservation status is negative, send a failure response to user
        if (equalIgnoreCase(airlineStatus, "Failed")) {
            outResponse.setJsonPayload({"Message":"Failed to reserve airline! " +
                    "Provide a valid 'Preference' for 'Airline' and try again"});
            var result = caller->respond(outResponse);
            handleError(result);
            return;
        }
        // CODE-SEGMENT-END: segment_2

        // CODE-SEGMENT-BEGIN: segment_3
        // Reserve hotel room for the user by calling Hotel reservation service
        // construct the payload
        json outReqJsonPayloadHotel = {
            Name: check outReqPayload.Name,
            ArrivalDate: check outReqPayload.ArrivalDate,
            DepartureDate: check outReqPayload.DepartureDate,
            Preference: hotelPreference
        };

        http:Request outReqPayloadHotel = new;
        outReqPayloadHotel.setJsonPayload(<@untainted>outReqJsonPayloadHotel);

        // Send a post request to hotelReservationService with appropriate payload and get response
        http:Response inResHotel = check hotelReservationEP->post("/reserve", outReqPayloadHotel);

        // Get the reservation status
        var hotelResPayload = check inResHotel.getJsonPayload();
        string hotelStatus = hotelResPayload.Status.toString();
        // If reservation status is negative, send a failure response to user
        if (equalIgnoreCase(hotelStatus, "Failed")) {
            outResponse.setJsonPayload({"Message":"Failed to reserve hotel! " +
                    "Provide a valid 'Preference' for 'Accommodation' and try again"});
            var result = caller->respond(outResponse);
            handleError(result);
            return;
        }
        // CODE-SEGMENT-END: segment_3

        // If all three services response positive status, send a successful message to the user
        outResponse.setJsonPayload({"Message":"Congratulations! Your journey is ready!!"});
        var result = caller->respond(outResponse);
        handleError(result);
        return ();
    }

}
```

Let's now look at the code segment that is responsible for parsing the JSON payload from the user request.

```ballerina
// Try parsing the JSON payload from the user request
        var payload = inRequest.getJsonPayload();
        if (payload is json) {
            // Valid JSON payload
            inReqPayload = payload;
        } else {
            // NOT a valid JSON payload
            outResponse.statusCode = 400;
            outResponse.setJsonPayload({"Message":"Invalid payload - Not a valid JSON payload"});
            var result = caller->respond(outResponse);
            handleError(result);
            return;
        }

        // Json payload format for an http out request
        json outReqPayload = {
            Name: check inReqPayload.Name,
            ArrivalDate: check inReqPayload.ArrivalDate,
            DepartureDate: check inReqPayload.DepartureDate,
            Preference:""
        };

        json airlinePreference = check inReqPayload.Preference.Airline;
        json hotelPreference = check inReqPayload.Preference.Accommodation;
        json carPreference = check inReqPayload.Preference.Car;

        // If payload parsing fails, send a "Bad Request" message as the response
        if (outReqPayload.Name == () || outReqPayload.ArrivalDate == () || outReqPayload.DepartureDate == () ||
            airlinePreference == () || hotelPreference == () || carPreference == ()) {
            outResponse.statusCode = 400;
            outResponse.setJsonPayload({"Message":"Bad Request - Invalid Payload"});
            var result = caller->respond(outResponse);
            handleError(result);
            return;
        }
```

The above code shows how the request JSON payload is parsed to create JSON literals required for further processing.

Let's now look at the code segment that is responsible for communicating with the airline reservation service.

```ballerina
// Reserve airline ticket for the user by calling Airline reservation service
        // construct the payload
        json outReqJsonPayloadAirline = {
            Name: check outReqPayload.Name,
            ArrivalDate: check outReqPayload.ArrivalDate,
            DepartureDate: check outReqPayload.DepartureDate,
            Preference: airlinePreference
        };

        http:Request outReqPayloadAirline = new;
        outReqPayloadAirline.setJsonPayload(<@untainted>outReqJsonPayloadAirline);

        // Send a post request to airlineReservationService with appropriate payload and get response
        http:Response inResAirline = check airlineReservationEP->post("/reserve", outReqPayloadAirline);

        // Get the reservation status
        var airlineResPayload = check inResAirline.getJsonPayload();
        string airlineStatus = airlineResPayload.Status.toString();
        // If reservation status is negative, send a failure response to user
        if (equalIgnoreCase(airlineStatus, "Failed")) {
            outResponse.setJsonPayload({"Message":"Failed to reserve airline! " +
                    "Provide a valid 'Preference' for 'Airline' and try again"});
            var result = caller->respond(outResponse);
            handleError(result);
            return;
        }
```

The above code shows how the travel agency service initiates a request to the airline reservation service to book a flight ticket. `airlineReservationEP` is the client endpoint you defined through which the Ballerina service communicates with the external airline reservation service.

Let's now look at the code segment that is responsible for communicating with the hotel reservation service.

```ballerina
// Reserve hotel room for the user by calling Hotel reservation service
        // construct the payload
        json outReqJsonPayloadHotel = {
            Name: check outReqPayload.Name,
            ArrivalDate: check outReqPayload.ArrivalDate,
            DepartureDate: check outReqPayload.DepartureDate,
            Preference: hotelPreference
        };

        http:Request outReqPayloadHotel = new;
        outReqPayloadHotel.setJsonPayload(<@untainted>outReqJsonPayloadHotel);

        // Send a post request to hotelReservationService with appropriate payload and get response
        http:Response inResHotel = check hotelReservationEP->post("/reserve", outReqPayloadHotel);

        // Get the reservation status
        var hotelResPayload = check inResHotel.getJsonPayload();
        string hotelStatus = hotelResPayload.Status.toString();
        // If reservation status is negative, send a failure response to user
        if (equalIgnoreCase(hotelStatus, "Failed")) {
            outResponse.setJsonPayload({"Message":"Failed to reserve hotel! " +
                    "Provide a valid 'Preference' for 'Accommodation' and try again"});
            var result = caller->respond(outResponse);
            handleError(result);
            return;
        }
```

The travel agency service communicates with the hotel reservation service to book a room for the client as shown above. The client endpoint defined for this external service call is `hotelReservationEP`.


## Testing 

### Invoking the service

- Navigate to `service-composition` and run the following command to start all three HTTP services. This starts the `Airline Reservation`, `Hotel Reservation` and `Travel Agency` services on ports 9091, 9092 and 9090 respectively.

```bash
   $ ballerina run guide
```
   
- Invoke the travel agency service by sending a POST request to arrange a tour.

```bash
   curl -v -X POST -d '{"Name":"Bob", "ArrivalDate":"12-03-2020",
   "DepartureDate":"13-04-2022", "Preference":{"Airline":"Business",
   "Accommodation":"Air Conditioned", "Car":"Air Conditioned"}}' \
   "http://localhost:9090/travel/arrangeTour" -H "Content-Type:application/json"
```

  Travel agency service sends a response similar to the following:
    
```bash
   < HTTP/1.1 200 OK
   {"Message":"Congratulations! Your journey is ready!!"}
``` 

## Deployment

Once you are done with the development, you can deploy the services using any of the methods that are listed below. 

### Deploying locally

- To deploy locally, navigate to `service-composition` directory, and execute the following command.
```bash
   $ ballerina build guide
```

- This builds a JAR file (.jar) in the target folder. You can run this by using the `java -jar` command.
```bash
  $ java -jar target/bin/guide.jar
```

- You can see the travel agency service and related backend services are up and running. The successful startup of services will display the following output. 
```
   [ballerina/http] started HTTP/WS listener 0.0.0.0:9091
   [ballerina/http] started HTTP/WS listener 0.0.0.0:9092
   [ballerina/http] started HTTP/WS listener 0.0.0.0:9090
```
