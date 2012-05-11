pyogcba
=======

Biblioteca de consulta del repositorio de datos de Gobierno Abierto de la Ciudad de Buenos Aires en python

Un proyecto con el cual busco aprender python y, en el medio, consultar la base de datos de Gobierno Abierto de GCBA.

Este proyecto NO esta afiliado con el GCBA de ninguna manera.

Funcionamiento de la biblioteca:
================================

En primer lugar, se basa en el modulo *ckanclient* para hacer las consultas de metadata al repositorio.

Luego, con la lista de datasets disponibles, se crea un objeto Dataset, que es cargado posteriormente.

Al cargarlo, el objeto descarga el contenido de todos los recursos del dataset a disco, y luego procede
a cargarlos en un diccionario en memoria para las consultas.

Una vez lista la informacion, se pueden realizar consultas al dataset mediante el metodo _query()_, que recibe
como parametros el nombre del recurso que se desea consultar y una funcion que hace el filtrado de las filas en el recurso.

Uso de la biblioteca:
=====================

Importando el modulo
```
>>> import opendata_gcba as od
```

Listando paquetes de datos disponibles
```
>>> ckan = od.get_ckan()
>>> package_list = ckan.package_register_get()
>>> package_list
[u'mapa-comunas', u'catalogo-centralizado-red-bibliotecas-publicas', u'mapa-establecimientos-educativos-privados', u'mapa-manzanas', u'obras-registradas', u'registro-fabricantes-reparadores-recargadores-matafuegos', u'presupuesto-sancionado-2010', u'empresas-seguridad-privada-habilitadas', u'registro-inspectores-verificadores', u'mapa-parcelas', u'api-mapa-interactivo', u'relevamiento-usos-suelo-2008', u'mapa-espacios-verdes', u'mapa-mediciones-rni', u'bafici', u'niveles-contaminacion-aire', u'mapa-bibliotecas', u'mapa-establecimientos-educativos-publicos', u'mapa-areas-hospitalarias', u'presupuesto-sancionado-2012', u'mapa-bicisendas', u'ejecucion-presupuestaria-2012', u'mapa-transformadores-pcb', u'presupuesto-sancionado-2007', u'presupuesto-sancionado-2011', u'presupuesto-sancionado-2009', u'mapa-calles', u'locales-bailables', u'bicicletas-publicas', u'mapa-autopistas', u'mapa-codigo-planeamiento-urbano', u'mapa-subterraneos', u'mapa-linea-ferrocarril', u'seguimiento-obras-adjudicadas-soa', u'visitas-web-gcba-2011', u'estado-autopista', u'mapa-distritos-escolares', u'mapa-barrios', u'sueldos-funcionarios', u'areas-proteccion-historica', u'presupuesto-sancionado-2006', u'presupuesto-sancionado-2005', u'presupuesto-sancionado-2008', u'mapa-comisarias', u'agenda-cultural', u'mapa-secciones-policiales', u'set-filmaciones', u'niveles-contaminacion-ruido', u'boletin-oficial']
```

Obteniendo un dataset
```
>>> bafici_dataset = od.Dataset(ckan, 'bafici')
>>> bafici_dataset.is_loaded()
False
>>> bafici_dataset.load()
```

Consultando que recursos fueron cargados desde un dataset
```
>>> [x[1] for x in bafici_dataset.get_available_dataset_keys()]
[u'bafici10-directores', u'bafici11-sedes', u'bafici12-paises', u'bafici11-paises', u'bafici11-programacion', u'bafici11-secciones', u'bafici10-sedes', u'bafici12-sedes', u'bafici12-directores', u'bafici12-films', u'bafici10-films', u'bafici11-directores', u'bafici10-secciones', u'bafici10-programacion', u'bafici12-secciones', u'bafici12-programacion', u'bafici10-paises', u'bafici11-films']
```

Consultando un dataset
```
>>>bafici_dataset.query('bafici12-sedes')
[{'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2009-03-06 12:17:43', 'weight': '2', 'title': 'Abasto Shopping', 'bus': '24-26-41-61-62-64-68-71-75-99-101-115-118-124-132-146-155-168-180- (R155)-188-194', 'img_photo': 'NULL', 'id_place': '1', 'filepic1': 'abasto_shopping_1.jpg', 'published': '1', 'phone': '0810-122-HOYTS (46987)', 'abbrev': '', 'train': '', 'subway': 'Subte l\xc3\xadnea B, Estaci\xc3\xb3n Carlos Gardel.', 'address': 'Av. Corrientes 3247', 'updated_ts': '2012-03-30 20:43:26', 'img_map': 'NULL', 'filemap1': 'abasto_shopping_1.jpg', 'rooms': 'HOYTS1,HOYTS2,HOYTS3,HOYTS4,HOYTS5,HOYTS6,HOYTS7,HOYTS8,HOYTS9,HOYTS10,HOYTS11,HOYTS12'}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2009-03-06 12:18:08', 'weight': '5', 'title': 'Atlas Santa Fe', 'bus': '10-12-29-37-39-59-60-75-99-101-102-106-108-109-111-124-140-152', 'img_photo': 'NULL', 'id_place': '2', 'filepic1': 'atlas_santa_fe_1.jpg', 'published': '0', 'phone': '', 'abbrev': '', 'train': 'NULL', 'subway': 'Subte l\xc3\xadnea D, Estaci\xc3\xb3n Pueyrred\xc3\xb3n.', 'address': 'Av. Santa Fe 2015', 'updated_ts': '2012-03-08 15:25:06', 'img_map': 'NULL', 'filemap1': 'atlas_santa_fe_1.jpg', 'rooms': 'Sta. Fe 1,Sta. Fe 2'}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2009-03-06 12:18:44', 'weight': '5', 'title': 'Teatro San Mart\xc3\xadn', 'bus': '5-6-7-23-24-26-29-39-50-56-60-64-86-99-102-105-109-111-115-124-140-142-146-151-180(R155)', 'img_photo': 'NULL', 'id_place': '3', 'filepic1': 'teatro_san_martin_1.jpg', 'published': '1', 'phone': '4371-0111', 'abbrev': '', 'train': '', 'subway': 'Subte l\xc3\xadnea B, Estaci\xc3\xb3n Uruguay. L\xc3\xadnea D, Estaci\xc3\xb3n Tribunales. L\xc3\xadnea A, Estaci\xc3\xb3n S\xc3\xa1enz Pe\xc3\xb1a', 'address': 'Av. Corrientes 1530 Piso 10\xc2\xba - Sala Leopoldo Lugones', 'updated_ts': '2012-03-23 17:07:33', 'img_map': 'NULL', 'filemap1': 'teatro_san_martin_1.jpg', 'rooms': 'Sala Leopoldo Lugones'}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2009-03-06 12:19:04', 'weight': '11', 'title': 'Alianza Francesa de Buenos Aires', 'bus': '5-7-9-10-17-23-29-39-45-59-67-70-75-99-100-101-102-106-108-109-111-115-132-140-150-152', 'img_photo': 'NULL', 'id_place': '4', 'filepic1': 'alianza_francesa_de_buenos_aires_1.jpg', 'published': '1', 'phone': '4322-0068', 'abbrev': '', 'train': '', 'subway': 'Subte l\xc3\xadnea C, Estaci\xc3\xb3n Lavalle. L\xc3\xadnea D, Estaci\xc3\xb3n Tribunales. L\xc3\xadnea B, Estaci\xc3\xb3n Carlos Pellegrini.', 'address': 'Av. C\xc3\xb3rdoba 946', 'updated_ts': '2012-03-21 19:21:51', 'img_map': 'NULL', 'filemap1': 'alianza_francesa_de_buenos_aires_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2009-03-06 12:19:23', 'weight': '9', 'title': 'Malba Cine', 'bus': '10-37-38-41-59-60-67-92-93-95-102-108-110-118-124-128-130', 'img_photo': 'NULL', 'id_place': '5', 'filepic1': 'malba_cine_1.jpg', 'published': '1', 'phone': '4808-6500 / 6515', 'abbrev': '', 'train': '', 'subway': '', 'address': 'Av. Figueroa Alcorta 3415', 'updated_ts': '2012-03-09 16:55:16', 'img_map': 'NULL', 'filemap1': 'malba_cine_1.jpg', 'rooms': 'Auditorio'}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2009-03-06 12:19:43', 'weight': '3', 'title': 'CCC Teatro 25 de Mayo', 'bus': '71-93-108-112-113-114-127-133-175-176', 'img_photo': 'NULL', 'id_place': '6', 'filepic1': 'ccc_teatro_25_de_mayo_1.jpg', 'published': '1', 'phone': '4524-7997', 'abbrev': '', 'train': '', 'subway': 'Subte l\xc3\xadnea B, Estaci\xc3\xb3n Los Incas', 'address': 'Av. Triunvirato 4444', 'updated_ts': '2012-03-22 13:58:56', 'img_map': 'NULL', 'filemap1': 'ccc_teatro_25_de_mayo_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2009-03-06 12:20:57', 'weight': '7', 'title': 'Anfiteatro del Parque Centenario', 'bus': '15,36,42,65,92,99,105,112,124,141', 'img_photo': 'NULL', 'id_place': '9', 'filepic1': 'anfiteatro_del_parque_centenario_1.jpg', 'published': '1', 'phone': '', 'abbrev': '', 'train': '', 'subway': 'L\xc3\xadnea B, estaci\xc3\xb3n \xc3\x81ngel Gallardo', 'address': 'Av. \xc3\x81ngel Gallardo y Leopoldo Marechal, entrada por Lillo', 'updated_ts': '2012-03-27 16:44:41', 'img_map': 'NULL', 'filemap1': 'anfiteatro_del_parque_centenario_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2010-03-17 23:17:18', 'weight': '8', 'title': 'Arteplex Belgrano', 'bus': '41- 57- 59-60-68-133-151-152-161-168- 169-175-184-194', 'img_photo': 'NULL', 'id_place': '12', 'filepic1': 'arteplex_belgrano_1.jpg', 'published': '1', 'phone': '4781-6500', 'abbrev': '', 'train': '', 'subway': 'L\xc3\xadnea D, Est. Congreso de Tucum\xc3\xa1n.', 'address': 'Av. Cabildo 2829', 'updated_ts': '2012-03-19 19:21:12', 'img_map': 'NULL', 'filemap1': 'arteplex_belgrano_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2010-03-20 19:00:26', 'weight': '3', 'title': 'BAFICITO al Aire Libre:  Plaza San Mart\xc3\xadn de Tours ', 'bus': '10-17-21-37-59-60-61-67-75-92-93-102-110-124-130', 'img_photo': 'NULL', 'id_place': '13', 'filepic1': 'baficito_al_aire_libre__plaza_san_martin_de_tours_1.jpg', 'published': '0', 'phone': '', 'abbrev': '', 'train': 'NULL', 'subway': '', 'address': 'Schiaffino y Posadas', 'updated_ts': '2012-03-08 15:26:48', 'img_map': 'NULL', 'filemap1': 'baficito_al_aire_libre__plaza_san_martin_de_tours_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2010-03-25 04:34:33', 'weight': '13', 'title': 'Abasto Shopping - Museo de los Ni\xc3\xb1os', 'bus': '24 - 26 - 41 - 61 - 62 - 64 - 68 - 71 - 75 - 99 - 101 - 115 - 118 - 124 - 132 - 146 - 155 - 168 - 18', 'img_photo': 'NULL', 'id_place': '15', 'filepic1': 'abasto_shopping_-_museo_de_los_ninos_1.jpg', 'published': '0', 'phone': '', 'abbrev': '', 'train': 'NULL', 'subway': 'Subte l\xc3\xadnea B, Estaci\xc3\xb3n Carlos Gardel.', 'address': 'Av. Corrientes 3247', 'updated_ts': '2011-03-09 21:45:30', 'img_map': 'NULL', 'filemap1': 'abasto_shopping_-_museo_de_los_ninos_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2011-03-09 21:55:08', 'weight': '10', 'title': 'Fundaci\xc3\xb3n Proa ', 'bus': '25, 29, 64, 86, 152. ', 'img_photo': 'NULL', 'id_place': '16', 'filepic1': 'fundacion_proa_1.jpg', 'published': '1', 'phone': '4104-1000', 'abbrev': '', 'train': '', 'subway': '', 'address': 'Av. Pedro de Mendoza 1929', 'updated_ts': '2012-03-09 17:15:30', 'img_map': 'NULL', 'filemap1': 'fundacion_proa_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2011-03-09 22:01:34', 'weight': '12', 'title': 'Cine Cosmos - UBA', 'bus': '5, 6, 7, 12, 23, 24, 26, 29, 37, 39, 50, 60, 102, 124, 146, 150, 155, 180 (R155) ', 'img_photo': 'NULL', 'id_place': '17', 'filepic1': 'cine_cosmos_-_uba_1.jpg', 'published': '1', 'phone': '4953-5405', 'abbrev': '', 'train': '', 'subway': 'Subte l\xc3\xadnea B, Estaci\xc3\xb3n Pasteur o Estaci\xc3\xb3n Callao', 'address': 'Av. Corrientes 2046', 'updated_ts': '2012-03-26 14:10:27', 'img_map': 'NULL', 'filemap1': 'cine_cosmos_-_uba_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2011-03-21 06:48:47', 'weight': '0', 'title': 'Universidad del Cine', 'bus': '', 'img_photo': 'NULL', 'id_place': '18', 'filepic1': 'universidad_del_cine_1.jpg', 'published': '0', 'phone': '', 'abbrev': '', 'train': 'NULL', 'subway': '', 'address': 'Pasaje J. M. Giufra 330', 'updated_ts': '2011-03-21 15:48:39', 'img_map': 'NULL', 'filemap1': 'universidad_del_cine_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2011-03-21 06:49:54', 'weight': '1', 'title': 'Punto de Encuentro BAFICI', 'bus': '', 'img_photo': 'NULL', 'id_place': '19', 'filepic1': 'punto_de_encuentro_bafici_1.jpg', 'published': '1', 'phone': '', 'abbrev': '', 'train': '', 'subway': '', 'address': 'Entrepiso del Abasto Shopping', 'updated_ts': '2012-03-21 14:07:38', 'img_map': 'NULL', 'filemap1': 'punto_de_encuentro_bafici_1.jpg', 'rooms': 'Auditorio'}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2011-03-22 20:14:29', 'weight': '0', 'title': 'Plaza Catalu\xc3\xb1a', 'bus': '', 'img_photo': 'NULL', 'id_place': '20', 'filepic1': 'NULL', 'published': '0', 'phone': '', 'abbrev': '', 'train': 'NULL', 'subway': '', 'address': 'Av. 9 de Julio, Av. Cerrito, Arroyo, Posadas.', 'updated_ts': '2011-03-22 20:14:29', 'img_map': 'NULL', 'filemap1': 'NULL', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2012-03-08 15:08:33', 'weight': '6', 'title': 'Centro Cultural San Mart\xc3\xadn', 'bus': '5, 6, 7, 23, 24, 26, 29, 38, 39, 50, 67, 102, 105, 142, 146, 155', 'img_photo': 'NULL', 'id_place': '21', 'filepic1': 'centro_cultural_san_martin_1.jpg', 'published': '1', 'phone': '4374-1251', 'abbrev': '', 'train': '', 'subway': 'L\xc3\xadnea B, estaci\xc3\xb3n Uruguay', 'address': 'Paran\xc3\xa1 esq. Sarmiento', 'updated_ts': '2012-03-23 16:36:38', 'img_map': 'NULL', 'filemap1': 'centro_cultural_san_martin_1.jpg', 'rooms': 'Sala 1,Sala 2'}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2012-03-08 16:34:50', 'weight': '4', 'title': 'Planetario de la Ciudad de Buenos Aires \xe2\x80\x9cGalileo Galilei\xe2\x80\x9d', 'bus': '12, 15, 29, 36, 37, 39, 41, 55, 57, 59, 60, 64, 67, 68, 93, 95, 102, 108, 111, 118, 124, 128, 130, 152, 160, 161, 166, 188.', 'img_photo': 'NULL', 'id_place': '23', 'filepic1': 'planetario_de_la_ciudad_de_buenos_aires_rgalileo_galileir_1.jpg', 'published': '1', 'phone': '4771-9393 / 6629', 'abbrev': '', 'train': 'L\xc3\xadnea Gral. San Mart\xc3\xadn, Est. Palermo. L\xc3\xadnea Gral. Belgrano, Est. 3 de Febrero. L\xc3\xadnea Mitre, Est. 3 de Febrero', 'subway': '', 'address': 'Av. Sarmiento y Belisario Rold\xc3\xa1n', 'updated_ts': '2012-03-12 17:27:02', 'img_map': 'NULL', 'filemap1': 'planetario_de_la_ciudad_de_buenos_aires_rgalileo_galileir_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2012-03-12 15:16:47', 'weight': '20', 'title': 'Casa de la Cultura', 'bus': '', 'img_photo': 'NULL', 'id_place': '24', 'filepic1': 'casa_de_la_cultura_1.jpg', 'published': '0', 'phone': '', 'abbrev': '', 'train': '', 'subway': '', 'address': 'Av. de Mayo 575', 'updated_ts': '2012-03-13 15:30:42', 'img_map': 'NULL', 'filemap1': 'casa_de_la_cultura_1.jpg', 'rooms': ''}, {'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2012-03-23 22:36:47', 'weight': '50', 'title': 'Fundaci\xc3\xb3n Universidad del Cine', 'bus': '', 'img_photo': 'NULL', 'id_place': '25', 'filepic1': 'fundacion_universidad_del_cine_1.jpg', 'published': '0', 'phone': '', 'abbrev': '', 'train': '', 'subway': '', 'address': 'Pasaje J. M. Giufra 330', 'updated_ts': '2012-03-23 22:37:06', 'img_map': 'NULL', 'filemap1': 'fundacion_universidad_del_cine_1.jpg', 'rooms': ''}]
```

Consultando una fila en particular
```
>>> bafici_dataset.query('bafici12-sedes')[0]
{'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2009-03-06 12:17:43', 'weight': '2', 'title': 'Abasto Shopping', 'bus': '24-26-41-61-62-64-68-71-75-99-101-115-118-124-132-146-155-168-180- (R155)-188-194', 'img_photo': 'NULL', 'id_place': '1', 'filepic1': 'abasto_shopping_1.jpg', 'published': '1', 'phone': '0810-122-HOYTS (46987)', 'abbrev': '', 'train': '', 'subway': 'Subte l\xc3\xadnea B, Estaci\xc3\xb3n Carlos Gardel.', 'address': 'Av. Corrientes 3247', 'updated_ts': '2012-03-30 20:43:26', 'img_map': 'NULL', 'filemap1': 'abasto_shopping_1.jpg', 'rooms': 'HOYTS1,HOYTS2,HOYTS3,HOYTS4,HOYTS5,HOYTS6,HOYTS7,HOYTS8,HOYTS9,HOYTS10,HOYTS11,HOYTS12'}
```

Corriendo una query
```
>>> bafici_dataset.query('bafici12-sedes', lambda r: r['title'].lower().find('galilei') != -1)
[{'city': 'Ciudad Aut\xc3\xb3noma de Buenos Aires', 'created_ts': '2012-03-08 16:34:50', 'weight': '4', 'title': 'Planetario de la Ciudad de Buenos Aires \xe2\x80\x9cGalileo Galilei\xe2\x80\x9d', 'bus': '12, 15, 29, 36, 37, 39, 41, 55, 57, 59, 60, 64, 67, 68, 93, 95, 102, 108, 111, 118, 124, 128, 130, 152, 160, 161, 166, 188.', 'img_photo': 'NULL', 'id_place': '23', 'filepic1': 'planetario_de_la_ciudad_de_buenos_aires_rgalileo_galileir_1.jpg', 'published': '1', 'phone': '4771-9393 / 6629', 'abbrev': '', 'train': 'L\xc3\xadnea Gral. San Mart\xc3\xadn, Est. Palermo. L\xc3\xadnea Gral. Belgrano, Est. 3 de Febrero. L\xc3\xadnea Mitre, Est. 3 de Febrero', 'subway': '', 'address': 'Av. Sarmiento y Belisario Rold\xc3\xa1n', 'updated_ts': '2012-03-12 17:27:02', 'img_map': 'NULL', 'filemap1': 'planetario_de_la_ciudad_de_buenos_aires_rgalileo_galileir_1.jpg', 'rooms': ''}]
```