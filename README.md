## Learn how to Connect|insert|Read| Write Bulk Data into Aurora Postgres using Python psycopg2

# How to use 
### Steps

##### Step1 : Install Library 
```
pip3 install -r requirements.txt
```
* Copy and paste the Classes into your Source or Main Python File


##### Step2 : Use Connector Object or Create instance 

```
helper = DatabaseAurora(
    data_base_settings=Settings(
        port=os.getenv("AURORA_DB_PORT"),
        server=os.getenv("AURORA_DB_SERVER"),
        username=os.getenv("AURORA_DB_UID"),
        password=os.getenv("AURORA_DB_PWD"),
        database_name=os.getenv("AURORA_DB_DATABASE"),
    )
)
```

##### OR
```
helper = DatabaseAurora(
    data_base_settings=Settings(
        port="XXXXXX",
        server="XXXXXX",
        username="XXXXXX",
        password="XXXXXX",
        database_name="XXXXXX",
    )
)
```


##### Step3 :Access All Methods in Class Easily 

##### Create table 
```
    query = """
                CREATE TABLE IF NOT EXISTS public.users
                (
                    first_name character varying(256) COLLATE pg_catalog."default",
                    last_name character varying(256) COLLATE pg_catalog."default",
                    address character varying(256) COLLATE pg_catalog."default",
                    text character varying(256) COLLATE pg_catalog."default",
                    id character varying(256) COLLATE pg_catalog."default",
                    city character varying(256) COLLATE pg_catalog."default",
                    state character varying(256) COLLATE pg_catalog."default"
                )
    """
    logger.logger.info("Creating Tables")
    response = helper.execute(query=query, data=None)
```

##### Bulk Inserts 
```
    query = """INSERT INTO public.users
                                        (
                                        first_name,
                                        last_name,
                                        address,
                                        text,
                                        id,
                                        city,
                                        state
                                        )
                                    VALUES (%s, %s, %s, %s, %s, %s,%s)"""
    

    data = [
        {'first_name': 'Jasmin', 'last_name': 'Cox', 'address': '21610 Guzman Burg\nNorth Daniel, CT 53547', 'text': 'International job so perhaps decide whether see. Water prove house direction may off. Which hotel rule guy base thing.', 'id': 'ffe6fb75-a0a5-4407-b35a-bf183d6940fc', 'city': 'North Sheilashire', 'state': 'Missouri'},
        {'first_name': 'Michael', 'last_name': 'Hubbard', 'address': '462 Kevin Harbor Apt. 593\nCodyfort, NH 20624', 'text': 'Dark these partner hope. Article doctor fine room star. Why without different control.\nForget into apply forget project when. Put look also deep air.', 'id': '21a3d9b9-534a-481a-8c79-db71c8aeded2', 'city': 'West Paul', 'state': 'Alabama'}
    ]
    values = [tuple(item.values()) for item in data]
    response = helper.insert_many(query=query, data=values)

    
```
##### Read Data in Paginated Manner  

```
    response = helper.get_data_batch(query="""SELECT * FROM public.users""", batch_size=100)
    while True:
        try:
            json_data = next(response)
            print(json_data)
        except Exception as e:
            print("Complete Reading ")
            break

```
##### Class Diagram 
![image](https://user-images.githubusercontent.com/39345855/198848086-17fa239d-3728-4616-b110-a690752bbe18.png)

