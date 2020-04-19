# AutoParkApi
REST API for automatic ticketing system for parking lot

## How to use
Following endpoints are available and can be run from docker (more details in Docker environment section).
Postman script is available [here](https://github.com/tchidk/AutoParkApi/blob/master/test/postman/ParkingLotApi.postman_collection.json). Example below for curl.
##### 1. Create parking lot
###### POST /parkinglot
<b>Parameters:</b>
- capacity : number - Capacity to parking lot

<b>Example:</b>
```
curl --location --request POST 'http://localhost:8080/parkinglot' \
--header 'Content-Type: application/json' \
--data-raw '{
 "capacity": 2
}'
```
<b>Return results: </b>
```
[
    {
        "Id": "5e9c264d4bbd460011f10b13",
        "SlotNumber": 1,
        "OccupyBy": null
    },
    {
        "Id": "5e9c264d4bbd460011f10b14",
        "SlotNumber": 2,
        "OccupyBy": null
    }
]
```

##### 2. Park the car (Allowed car size: SS, S, M, L, XL, XXL)
###### PUT /parkinglot/park
<b>Parameters:</b>
- plateNumber:string - Plate number of the car
- size: string - Size of the car (SS, S, M, L, XL, XXL)

<b>Example:</b>
```
curl --location --request PUT 'http://localhost:8080/parkinglot/park' \
--header 'Content-Type: application/json' \
--data-raw '{
 "plateNumber": "AB-0001",
 "size": "S"
}'
```
<b>Return results: </b>
```
{
    "Id": "5e9c264d4bbd460011f10b13",
    "SlotNumber": 1,
    "OccupyBy": {
        "PlateNumber": "AB-0001",
        "Size": "S"
    }
}
```
##### 3. The car leaves from parking lot. Specify car's plate number to free the slot occupied by this car.
###### PUT /parkinglot/leave
<b>Parameters:</b>
- plateNumber:string - Plate number of the car

<b>Example:</b>

```
curl --location --request PUT 'http://localhost:8080/parkinglot/leave' \
--header 'Content-Type: application/json' \
--data-raw '{
 "plateNumber": "AB-0001"
}'
```
<b>Return results: </b>
```
{
    "Id": "5e9c264d4bbd460011f10b13",
    "SlotNumber": 1,
    "OccupyBy": null
}
```
##### 4. Get parking lot status
###### GET /parkinglot
<b>Parameters: </b>
- NA

<b>Example:</b>
```
curl --location --request GET 'http://localhost:8080/parkinglot'
```
<b>Return results: </b>
```
[
    {
        "Id": "5e9c264d4bbd460011f10b13",
        "SlotNumber": 1,
        "OccupyBy": {
            "PlateNumber": "AB-0001",
            "Size": "S"
        }
    },
    {
        "Id": "5e9c264d4bbd460011f10b14",
        "SlotNumber": 2,
        "OccupyBy": null
    }
]
```
##### 5. Get list of plate number by car size (Allowed car size: SS, S, M, L, XL, XXL)
###### GET /parkinglot/parked/plateNumber

<b>Parameters:</b>
- carSize: string - Size of the car (SS, S, M, L, XL, XXL)

<b>Example:</b>

```
curl --location --request GET 'http://localhost:8080/parkinglot/parked/plateNumber?carSize=S'
```
<b>Return results:</b>
```
[
    "AB-0001",
    "AB-0002"
]
```
##### 6. Get list of parking slot by car size (Allowed car size: SS, S, M, L, XL, XXL)
###### GET /parkinglot/parked/slotNumber

<b>Parameters:</b> 
- carSize: string - Size of the car (SS, S, M, L, XL, XXL)  

<b>Example:</b>
```
curl --location --request GET 'http://localhost:8080/parkinglot/parked/slotNumber?carSize=S'
```
<b>Return results:</b>
```
[
    1,
    2
]
```

### Development

```bash
$ npm i
$ npm run start:watch
$ open http://localhost:8080/
```
For development with local MongoDB, run `docker-compose up` for MongoDB and Mongo express, then stop node container and run node for development instead. Also Make sure that MongoDB connection is connect to `localhost` when run node for development, refer to [source code here](https://github.com/tchidk/AutoParkApi/blob/master/src/loaders/mongoose.ts#L6). This need to be moved to environment configuration in the future.

### Test

```bash
$ npm run test
```
OR with coverage report
```bash
$ npm run test-dev
```

### Docker environment

```bash
$ docker-compose up
```
Note: Make sure that MongoDB connection is connect to `mongo` instead of `localhost` refer to [source code here](https://github.com/tchidk/AutoParkApi/blob/master/src/loaders/mongoose.ts#L6) . This need to be moved to environment configuration in the future.