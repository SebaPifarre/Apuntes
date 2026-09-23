
* Hay que agregar el parámetro env=None en el create app, lo del static folder ya está
* Bajo /src agrega un config.py (min 32 aprox)
	* Agrega un clase Config
	* Agrega una clase DevelopmentConfig
	* Agrega una clase ProductionConfig
	* Agrega una clase TestingConfig
	* Agrega un diccionario
* Desde el main.py (o el init?) importa el config para cambiar el env del create_app (min 39)
* 