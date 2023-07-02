# Consultas SPARQL

## + Obligatorias

### ¿Cuáles son los inmuebles con superficie de lote mayor a una cantidad de metros cuadrados, que se encuentre en un barrio con servicios de luz, gas y cloacas?

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>

SELECT ?inmueble ?barrio ?m2Lote
WHERE {
  VALUES ?m2Paramentro {40}
  VALUES ?tiposInmuebles :Estate

   ?inmueble a ?tiposInmuebles;
      :isContained ?lote.

  ?lote a :Lot;
      :inNeighborhood ?barrio ;
              :m2 ?m2Lote.

  ?barrio a :Neighborhood;
      :hasService :Luz, :Gas, :Cloacas.


  FILTER (?m2Lote > ?m2Paramentro)

}
```

---

### ¿Qué inmobiliaria posee inmuebles para un perfil de vivienda dado?

**Perfiles:**

- Departamento con terraza, en piso 3, con servicio de agua y luz:

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>

SELECT ?inmueble ?barrio ?m2Lote
WHERE {
    VALUES ?m2Paramentro {40}

    ?inmueble a :Estate;
        :isContained ?lote.

    ?lote a :Lot;
        :inNeighborhood ?barrio ;
                :m2 ?m2Lote.

    ?barrio a :Neighborhood;
        :hasService :Luz, :Gas, :Cloacas.

    FILTER (?m2Lote > ?m2Paramentro)
}
```

- Casa de 2 pisos, mas de 3 habitaciones, mas de 1 baño, con balcon.

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?inmobiliaria ?aviso ?casa (count(distinct ?habitaciones) as ?cantidadHab) (count(distinct ?baños) as ?cantidadBaños)
WHERE {
    VALUES ?numeroPisos { "2"^^xsd:positiveInteger }

    ?inmobiliaria a :RealEstate;
              :publisher_of ?aviso.

    ?aviso a :RealEstatePublication;
              :aboutBuilding ?casa.

    ?casa a :House;
              :numberOfStoreys ?numeroPisos;
              bot:hasElement ?balcon;
              bot:containsZone ?habitaciones;
              bot:containsZone ?baños.

    ?balcon a :Balcony.

    ?habitaciones a :Bedroom.

    ?baños a :Bathroom.
}
GROUP BY ?casa ?inmobiliaria ?aviso
HAVING (?cantidadHab > 3 && ?cantidadBaños > 1)
```

- Casas con jardin y pileta

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?inmobiliaria ?aviso ?casa
WHERE {
     ?inmobiliaria a :RealEstate;
                :publisher_of ?aviso.

      ?aviso a :RealEstatePublication;
                :aboutBuilding ?casa.

      ?casa a :House;
                bot:hasElement ?pileta;
                bot:hasSpace ?jardin.

    ?pileta a :Pool.

    ?jardin a :Yard
}
```

- Galpon con mas de 50 m2 cubiertos, y con sercicio de luz

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?inmobiliaria ?aviso ?galpon ?m2Cubiertos
WHERE {
    VALUES ?m2Pedidos { 50 }

    ?inmobiliaria a :RealEstate;
                :publisher_of ?aviso.

    ?aviso a :RealEstatePublication;
                :aboutBuilding ?galpon.

    ?galpon a :Shed;
                :coveredM2 ?m2Cubiertos;
                bot:hasSpace ?jardin.

    ?jardin a :Yard.

    FILTER ( ?m2Cubiertos > ?m2Pedidos)
}
```

## + Extras

### Vecinos de un departamento (para mostrar transitividad)

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?depVecinos
WHERE {

    ?depVecinos :neighborInFloor :Departamento1P3EL1

}
```

### Avisos sobre inmuebles que acepten creditos hipotecarios y que tengan un precio menor a 100000 dolares

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?inmobiliaria ?aviso ?inmueble ?precioInmueble
WHERE {
    VALUES ?precioMaximo { "100000"^^xsd:float }

    ?inmobiliaria a foaf:Agent;
            :publisher_of ?aviso.

    ?aviso a :RealEstatePublication;
            :sellerAcceptsMortgageLoan "true"^^xsd:boolean;
            :price ?precioInmueble;
            :currency "dolares";
            :aboutBuilding ?inmueble.

    ?inmueble a :Estate;
            :availableForMortgageLoan "true"^^xsd:boolean;

    FILTER ( ?precioMaximo > ?precioInmueble )
}
```

### Avisos que permitan realizar créditos hipotecarios sobre un inmueble especifico y que tengan un precio menos a 100000 dolares

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?inmobiliaria ?aviso ?inmueble ?precioInmueble
WHERE {
    VALUES ?precioMaximo { "10000000"^^xsd:float }

    ?inmobiliaria a foaf:Agent;
                :publisher_of ?aviso.

        ?aviso a :RealEstatePublication;
            :sellerAcceptsMortgageLoan "true"^^xsd:boolean;
            :price ?precioInmueble;
            :currency "dolares";
            :aboutBuilding :CasaL6.

      FILTER ( ?precioMaximo > ?precioInmueble )
}
```

### Avisos relacionados a Departamentos/Lotes/Casas/Edificios

```sparql
PREFIX : <http://www.semanticweb.org/david/ontologies/2023/5/real_state_offer/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX bld: <http://bimerr.iot.linkeddata.es/def/building#>
PREFIX bot: <https://w3id.org/bot#>
PREFIX xml: <http://www.w3.org/XML/1998/namespace>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?aviso ?inmueble
WHERE {
    ?aviso a :RealEstatePublication;
    :aboutBuilding ?inmueble.

    ?inmueble a :House
}
```
