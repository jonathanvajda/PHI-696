**The SPARQL Library of Buffalo**

[Codewars](https://www.codewars.com/dashboard) is a website designed to facilitate algorithmic training for various programming languages. Users supply problem statements and others provide coding solutions to those problems. For example, you might find a problem for Python such as: 

**Jonathan's Kata's for SPARQL**

Comment: I've attempted 25 problems.
 - Kata-8's x 1 = 0
 - Kata-7's x 8 = 16
 - Kata-6's x 7 = 21
 - Kata-5's x 5 = 25
 - Kata-4's x 4 = 40
(this would be a total of 102 points, though if one of my Kata-4's is actually a Kata-3 please let me know!)

My problems are given in the following format: "Problem [Kata level]-[instance]"

**Problem 8-1.**
- The following catalog contains camera products, according to their product name (ontocam:productName), model number (ontocam:modelValue), manufacturer (ontocam:manufacturer) and release date (ontocam:releaseDate).
- Find all camera products manufactured by Canon, displaying their name, model number, release date.

```
PREFIX ontocam: <https://aperturescience.org/onto/>

SELECT ?productName ?modelValue ?manufacturer ?releaseDate
WHERE {
      ?device ontocam:manufactured_by ontocam:Canon .
}
```

**Problem 7-1.**
- List all of the people who have been president in the United States of America. The ontology does not have a 'president' class, but rather a 'PresidentRole' that the individual bears. Be careful to stipulate that the role has presidential authority in the United States. Role instances are automatically named by their order. Display in order of presidential role, from first to last.
- Who has been a president of the USA at some time?

```
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX ontopol: <https://politicalontology.org/schema/ontopol.ttl>

SELECT ?subject
WHERE {
      ?PresidentialRole ro:has_bearer ?subject ;
		a ontopol:PresidentialRole ;
		ontopol:authority_in ontopol:UnitedStates .
      ORDER BY ASC(?PresidentialRole)
    }
```

**Problem 7-2.**
- List all of the furniture by name that has been recalled for reason of "Child Safety". In this RDF model, all furniture has an active safety status of "Approved" or "Recalled" (they do not have a null value). Display by name in alphabetical order. Guidance for variables is as follows:
- *furnont* classes, relations, and properties:
	- furniture : type
	- commonName : string value
	- has_safety_status : range expects a literal
	- recalled_reason : range expects a literal
```
PREFIX furnont: <https://fillingbetterhomes.org/oit/furnitureontology.owl>

SELECT ?commonName ?safetyStatus
WHERE {
      ?furniture rdfs:label ?commonName ;
      		rdf:type ?furnont:Furniture ;
            furnont:has_safety_status "Recalled" ;
            furnont:recalled_reason "Child Safety" .
	ORDER BY ASC(?commonName)
    }
```

**Problem 7-3.**
- A local militia group wants to rally potential troops for a coup and has access to the state of Oregon registry to look up demographic information. They only want to find males age 16 and older.
- Find all males’ 16 years or older, and their mailing addresses.
```
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX ontopol: <https://politicalontology.org/schema/ontopol.ttl>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT ?person ?mbox
WHERE {
      ?person a ontopol:Person ;
		foaf:age ?age ;
		foaf:gender “male” ;
		foaf:mbox ?mbox .
      FILTER (?age > "15")
}
```

**Problem 7-4.**
- A tour guide in western Michigan wants to create a new lighthouses tour, based on lighthouses that provide support for boats on Lake Michigan, but from Michigan's side and only on the lower peninsula. Group by county, and order alphabetically by county.
- Lighthouses overlooking Lake Michigan, located in Michigan’s Lower Peninsula.
```
PREFIX touronto: <https://americathebeautiful.com/tours/ontology/>

SELECT ?facility ?county
WHERE {
	?facility rdf:type touronot:Lighthouse ;
		touronto:located_in touronto:LowerPeninsulaMichigan ;
		touronto:located_in ?county ;
		touronto:nearby_location touronto:LakeMichigan .
	GROUP BY ?county
	ORDER BY ASC(?county)
}
```

**Problem 7-5.**
- The National Park and Recreation Department is concerned about recent adverse changes in natural habitat for the ruby-throated thrush. Find all parks that may be affected by these conservation efforts. Group the results by state, and then display them in alphabetical order by county.
- Return all parks that are the natural habits of ruby-throated thrush.
```
PREFIX bfo: <http://purl.obolibrary.org/obo/bfo.owl>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX touronto: <https://americathebeautiful.com/tours/ontology/>

SELECT ?park ?county ?state
WHERE {
	?park a touronto:GeographicalPark ;
		ro:located_in ?site ;
		ro:located_in ?county ;
		ro:located_in ?state .
	?site a bfo:site ;
		touronto:contains_habitat ?naturalHabitat .
	?naturalHabitat a touronto:NaturalHabitat ;
		touronto:habitat_of zoo:rubyThroatedThrush .
	?county a touronto:County .
	?state a touronto:State .
	GROUP BY ?state
	ORDER BY ASC(?county)
}
```

**Problem 7-6.**
- Find all cameras manufacturerd by Fujifilm (ontocam:manufacturer), released in the year 2020, and has a bluetooth connection function. Return only the first 25.
- Return camera name (ontocam:productName), model number (ontocam:modelValue), release date (ontocam:releaseDate).
```
PREFIX ontocam: <https://aperturescience.org/onto/>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

SELECT ?productName ?modelValue ?releaseDate
WHERE {
	?device a ontocam:Photocamera
		ontocam:manufactured_by ontocam:Fujifilm ;
		ontocam:released_in_year "2020" ;
		ro:has_function ?Bluetooth .
	?Bluetooth a ontocam:BluetoothConnectionCapability .
	?productname rdfs:label ?label
	LIMIT 25
}
```

**Problem 7-7.**
- A catalog should display all lighting fixture models that have a multi-brightness function and can be connected as an Internet of Things device (IoT).
- Return the product name, product model, current price, and current inventory that fit this criteria. Display them by number of inventory from most to least.

```
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX homedev: <http://homeandgarden.com/ontology/>

SELECT ?productName ?productModelValue ?productPriceUSD ?inventoryValue
WHERE {
        ?product ro:instance_of homedev:LightingFixture ;
                ro:has_function homedev:multiSettingBrightness ;
                ro:has_function homedev:internetOfThingsConnection .
	ORDER BY DESC(?inventoryValue)
}
```

**Problem 7-8.**
- A sample ballot for a local county should display all candidates who are running in an upcoming election. Display alphabetically by party affiliation.
- Return the candidate name, candidate's political affiliation, and candidate's website address.
```
PREFIX ontopol: <https://politicalontology.org/schema/ontopol.ttl>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

SELECT ?PreferredName ?RegisteredPoliticalParty ?OfficialWebsiteURL
WHERE {
        ?ontopol:Citizen ro:referred_by ?ontopol:PreferredName ;
		ro:bearer_of ontopol:PoliticalCandidateRole ;
		ontopol:affiliated_with ?ontopol:RegisteredPoliticalParty ;
		ontopol:platform_described_by ?OfficialWebsiteURL .
	ORDER BY ASC(?RegisteredPoliticalParty)
}
```

**Problem 6-1.**
- You’re an IT administrator working with a printer support maintenance contractor PrinTechSolutions (itdev:PrinTechSolutions) to update and replace parts for covered assets (itdev:covered_by) under some maintenance contract (itdev:maintenanceContract) such as the Brother brand printers on site. You want to prioritize those printers that are plugged-in and active on the network (has_network_status takes values either "UP", "DOWN", or "UNKNOWN") and display a warning status (itdev:warningStatus), especially for “Low” toner (takes a literal value). Display the device in order of building room number (itdev:officeLocation) in descending order. Since this query will only return the highest priority for a given day and may be run repeatedly, return only the first 10.
- Get all printers on the network that have model made by Brother and which have a warning status on “High” (takes a literal value), optional: get their toner level status.
```
PREFIX itdev: <https://www.rando.org/ontology/>
 
SELECT ?printerDevice ?warningStatus ?officeLocation
WHERE {
	?printer itdev:manufactured_by itdev:Brother ;
		itdev:device_status ?warningStatus .
		itdev:covered_by ?maintenanceContract .
		itdev:has_network_status "UP" .
	?maintenanceContract itdev:has_contractor itdev:PrinTechSolutions .
	?warningStatus itdev:severity_level “High” . 
	OPTIONAL SELECT { ?printer itdev:toner_value “Low” . }
	ORDER BY DESC(?officeLocation)
	LIMIT 10
}
```

**Problem 6-2.**
- A store manager is assessing what Summer furniture needs to be put on sale before the next season. First, she is interested only in those furniture that is designed for use outdoors. Second, she wants to start with the type that has the most inventory. Third, she wants to limit the sale to the top 20 furniture models, irrespective of brand or purpose. Fourth, she wants the manufacturer's suggested retail price (MSRP). Lastly, she wants to know what the price would be if that product were 40% off the MSRP.
- Return the furniture name, model, and current inventory, ordered by inventory descending, but only the twenty products with the greatest inventory.
```
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX homedev: <http://homeandgarden.com/ontology/>

SELECT ?productName ?productModelValue ?inventoryValue ?productMSRP ?tentativeSalePrice
WHERE {
  ?product ro:instance_of homedev:outdoorFurniture .
  ?product ro:has_value homedev:inventoryValue .
  BIND (?productMSRP * 0.8 AS ?tentativeSalePrice)
}
```

**Problem 6-3.**
Find all constructed languages present in written works, that are not Star Wars lore, and what those works are.
```
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX ontolit: <http://litararyworlds.org/literature-ontology/ontolit.ttl>

SELECT ?conLang ?literaryArtifact
WHERE {
        ?sentence ontolit:in_language ?conLang ;
            ro:part_of ?litraryArtifact .
        ?litraryArtifact ontolit:has_literary_world ?literaryWorld .
  FILTER !regext(?litraryArtifact ro:has_literary_world ontolit:StarWarsUniverse)
}
```

**Problem 6-4.**
- Find all persons who have a passport issued, the person has not left the country, and the person is over the age of 25
```
PREFIX ontopol: <https://politicalontology.org/schema/ontopol.ttl>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

SELECT ?preferredName
WHERE {
        ?person ro:referred_by ?ontopol:preferredName ;
            ontopol:issued_document ?ontopol:Passport .
        FILTER (?age > 25) .
        FILTER NOT EXISTS (?person ro:participates_in ontopol:ActOfCountryDeparture)
}
```

**Problem 6-5.**
- List all senators or congresspersons who held office in a state that is not the state of their birthplace. Return both birthplace state and state in which they held office.
- This query calls for a disjunction. Be sensitive to what is being negated.
```
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX ontopol: <https://politicalontology.org/schema/ontopol.ttl>

SELECT * WHERE
{
{ SELECT ?person ?birthState ?officeState WHERE {
      ?person ro:has_role ?SenatorRole ;
      ?SenatorRole a ontopol:politicalOfficerRole ;
      ?politicalOfficerRole ontopol:authority_in ?State ;
      BIND(?State AS ?officeState) .
      ?person ontopol:has_birthplace ?State ;
      BIND(?State AS ?birthState) .
      }
}
UNION
{ SELECT ?person ?birthState ?officeState WHERE {
      ?person ro:has_role ?congressPersonRole ;
      ?congressPersonRole a ontopol:politicalOfficerRole ;
      ?politicalOfficerRole ontopol:authority_in ?State ;
      BIND(?State AS ?officeState) .
      ?person ontopol:has_birthplace ?State ;
      BIND(?State AS ?birthState) .
      }
}
FILTER(?birthState != ?officeState)
```

**Problem 6-6.**
- Find all cars manufactured outside the USA, have 360 camera sensors, and rated by JD Power & Associates as having the highest rating in its class. Group by body type.
```
PREFIX ontomobile: <http://carscarscars.org/schemas/ontology/ontomobile.owl>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

SELECT ?Vehicle ?VehicleBodyType
WHERE {
        ?Vehicle ontomobile:ro:manufactured_by ?Manufacturer ;
            ro:has_part ?360CameraSystem ;
            ontomobile:has_body_type ?VehicleBodyType ;
            ontomobile:has_quality_rating ?QualityRating .
        FILTER MAX(?QualityRating ontomobile:rated_by ontomobile:JDPowerAndAssociates ) .
        GROUP BY ?VehicleBodyType .
}
```


**Problem 6-7.**
- Find all musicians born in 1960s, who were members of some grunge band that was based in Seattle.
```
PREFIX ontomuse: <https://openmusicresources.org/schema/ontomuse.owl>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

SELECT ?Name
WHERE {
        ?ontomuse:Person ro:referred_by ?ontomuse:Name ;
            foaf:birthday ?birthDate ;
            ro:member_of ?MusicalGroup .
        ?MusicalGroup ontomuse:has_genre ontomuse:Grunge ;
            ontomuse:has_music_scene_locale ontomuse:Seattle_WA_USA .
        FILTER (?foaf:birthdate > "1960-01-01T00:00:00+05:30"^^xsd:dateTime) .
        FILTER (?foat:birthdate < "1970-01-01T00:00:00+05:30"^^xsd:dateTime)
}
```


**Problem 5-1.**
- A customer is searching for shelving and wants to filter results in a catalog online, with specific functionality and material type and within a price range.
- Get the USD price for all shelves that are not two-shelf, that cost less than $150 USD, and are made out of wood rather than wire frame.
```
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX homedev: <http://homeandgarden.com/ontology/>

SELECT ?tallShelf ?salePriceValueUSD
WHERE {
  ?shelf ro:instance_of homedev:Shelf ;
           ro:bearer_of ?function ;
           ro:composed_primarily_of ?material ;
           homedev:has_sale_price ?salePriceValueUSD .
  FILTER (?function != homedev:twoShelf && 
          ?salePriceValueUSD < 150 && 
          ?material = homedev:woodMaterial && 
          ?material != homedev:metalwireMaterial)
}
```

**Problem 5-2.**
- The federal department of education gives special grants to those educational institutions that garner the highest enrollments in the state (only one per state), whether that school is public or private. The total enrollment includes both undergraduate and postgraduate students. Find the college or university that would be eligible for such a grant in the state of California. Assert that the institution has the topEnrollment number and that it is a member of an eligible institution list, using CONSTRUCT regarding bearing a role (itdev:EligibleForFederalGrant).
- Give the name of the institution and number of students of a post-secondary educational institution located in California with the highest enrollment of students (undergrad and grad together).
```
PREFIX onto-ed: <https://www.nj.gov/admin/oit/ontology/>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

CONSTRUCT { ?institution onto-ed:enrollment_number ?topEnrollment ;
		ro:bearer_of ?EligibleForFederalGrant . }
WHERE {
	?institution a onto-ed:EducationalInstitution ;
		onto-ed:type "PostsecondaryEducation" ;
		ro:has_location onto-ed:California ;
		onto-ed:enrollment_number ?postgradEnrollment ;
		onto-ed:enrollment_number ?undergradEnrollment ;
	BIND (?postgradEnrollment + ?undergradEnrollment AS ?totalEnrollment) .
	BIND ( MAX(?totalEnrollment)) AS ?topEnrollment .
}
```

**Problem 5-3.**
- Your goal is to register a block of unused VINs. Take fields from multiple tables to generate a unique VIN. The third field is a check-digit, which in this case will be "9". Your manufacturing plant will only be assigning 1000, starting at sequence number 31337.
- The order of VIN construction:
	- WMI (ontomobile:WorldManufacturerIdentifier)
	- Vehicle attributes (ontomobile:VehicleAttributeCode)
	- Check digit
	- Model year (ontomobile:VehicleModel (a subclass of ArtifactModel))
	- Plant code (ontomobile:ManufacturerPlantCode)
	- Sequential number (ontombile:VehicleSequenceValue)
```
PREFIX ontomobile: <http://carscarscars.org/schemas/ontology/ontomobile.owl>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

CONSTRUCT REDUCED { ?Vehicle ontomobile:has_serial ?VehicleIdentificationNumber }
WHERE {
		?Vehicle ontomobile:designated_by ?VehicleSequenceValue ;
			ontomobile:manufactured_by ?Manufacturer ;
			ontomobile:manufactured_at_location ?ManufactuingPlant ;
			ontomobile:prescribed_by ?ArtifactModel ;
			ontomobile:described_by ?VehicleAttributeCode ;
			ontomobile:has_production_year ?ProductionYear .
		?Manufacturer ontomobile:has_WMI_value ?WorldManufacturerIdentifier .
		?ManufacturingPlant ontomobile:has_plant_code ?ManufacturerPlantCode .
		FILTER (?VehicleSequenceValue > "31336")
		ORDER BY ?VehicleSequenceValue ASC
        LIMIT 1000
		BIND(CONCAT(?WorldManufacturerIdentifier, ?VehicleAttributeCode, STR(9), ?VehicleModel, ?ProductionYear ?ManufacturerPlantCode, ?VehicleSequenceValue)) AS ?VehicleIdentificationNumber
}
```

**Problem 5-4.**
- Find all public elections for president held in sovereign states that were formerly members of the USSR and not currently dissolved. Display the country and the winner as well. Declare that state to bear a "NATO friend" role.
- Note: be sensitive to the open world assumption.
```
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX ontopol: <https://politicalontology.org/schema/ontopol.ttl>

CONSTRUCT { ?country ro:bearer_of ?ontopol:NATOFriendRole . }
WHERE {
		?election a ontopol:Public_Election ;
			ontopol:candidate_office ontopol:President ;
			ro:located_in ?country .
		?country ontopol:member_of_union ontopol:Soviet_Union ;
			ro:has_status ?status .
		FILTER (?status != "Dissolved") .
}
```

**Problem 5-5.**
- Suppose that new research shows that males are at heightened risk for testicular cancer if they have the conjunction of two genes, erbB-2 (Receptor tyrosine-protein kinase erbB-2) and PMS2 (Mismatch repair endonuclease PMS2). However, those whose mitochondrial haplogroup L0, L1, L2, L4, L5, L6 (i.e., L1-L6, except L3) are not yet known to be significantly affected. Notifications must be sent out about the incidental findings to patients who have received DNA testing. So clinicians want to tag all patients in their database as having a "cancer vulnerability role" when they meet the aforementioned criteria. Assert two triples that reflects this new information about such persons: the person has a predisposition to testicular cancer, and the person bears a cancer vulnerability role.
```
PREFIX cco: <http://www.ontologyrepository.com/CommonCoreOntologies/>
PREFIX ontokrebs: <http://purl.obolibrary.org/obo/krebs.owl>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

CONSTRUCT {
	?Person ontokrebs:has_predisposition ?TesticularCancer .
	?Person ro:bearer_of ?CancerVulnerabilityRole .
	}
WHERE {
	?Person ontokrebs:has_DNA_sequence ontokrebs:Gene_erbB2 ;
		ontokrebs:has_DNA_sequence ontokrebs:Gene_PMS2 ;
		ontokrebs:has_chromosomal_sex ontokrebs:Male .
		ontokrebs:has_haplogroup_family ?Haplogroup .
	FILTER !regex(?Haplogroup = "L0"|"L1"|"L2"|"L4"|"L5"|"L6" )
}
```

**Problem 4-1.**
- Around the world there are large buildings that bear the same shape as the famous Egyptian pyramids. In Mesopotamia, these are called ziggurats. However, Egyptians were not the first to build this kind of structure. For our purposes, all ziggurats and pyramids are pyramidesque buildings (pyramidesqueBuilding), even if the database does not say so. Which pyramid structures have the greatest estimated age?
- Display the name of the oldest 25 ziggurats or pyramids, current country location, name of associated culture when it was formed, and estimated age. Optionally, display whether it was a religious building, and display as artifact of that religion. Order by estimated age, descending (or date of completion ascending).
- HINT 1: You may use Construct to assert that all ziggurats are pyramids, or bind the disjunction to create a temporary class.
- HINT 2: Since hundreds have been created since AD 1500, make sure you filter so that the query can complete. 
```
PREFIX anthro: <http://anthropologywiki.org/schema/ontology/>

SELECT ?pyramidsqueBuilding ?countryName ?cultureName ?estimatedCompletionDate ?religiousArtifact
WHERE {
	?Building anthro:location_in ?Country ;
		anthro:cultural_originator ?Culture ;
		anthro:date_completed ?EstimatedCompletionDate .
	?Country rdfs:label ?countryName .
	?Culture rdfs:label ?cultureName .
	BIND (?ziggarat|?pyramid) AS ?pyramidesqueBuilding
	OPTIONAL { ?Building anthro:religious_site_of ?Religion .
  		BIND exists(?Building anthro:religious_site_of ?Religion) AS ?ReligiousArtifact }
	FILTER ( ?EstimatedCompletionDate < 1500-01-01 )
}
ORDER BY ASC(?estimatedCompletionDate)
LIMIT 25
```


**Problem 4-2.**
- An E. coli breakout has occurred recently and the culprit is contaminated iceberg lettuce pallets in the distribution for fastfood restaurants in the midwestern states. All pallets of lettuce that passed through distribution chains in Arkansas and Nebraska are at risk. Declare all such lettuce to bear an "E. coli risk" role and "recall" quality status, if they were present on shipping routes through these two states. (You must change the safety rating, and then assert a new triple that the pallet bears a risk role with CONSTRUCT or a similar function)
```
PREFIX foodservont: <http://industrialontologyfoundry.org/ontology/extension/foodservice.owl>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>

DELETE {
	?Pallet foodservont:has_quality_rating ?FoodSafetyRating . 
	?FoodSafetyRating ro:has_value "Safe" . 
	}
INSERT {
	?Pallet foodservont:has_quality_rating ?FoodSafetyRating . 
	?FoodSafetyRating ro:has_value "Recall" .
	}
CONSTRUCT {
	?Pallet ro:bearer_of ?EscherichiaColiRiskRole . 
	}
WHERE {
	?Pallet foodservont:contains_food_stuff ?Lettuce ;
		ro:participates_in ?ShippingProcess .
	?ShippingRoute ro:participates_in ?ShippingProcess ;
		ro:has_part ?ShippingStop .
	?ShippingStop ro:has_location {foodservant:Nebraska_USA|foodservant:Arkansas_USA} .
}
```

**Problem 4.3 ... or Kata-3?**
- A research company is working on anemia treatment. They belong to a network of research organizations permitted to make requests from biological banks that contain specimens available for "secondary research," i.e., excess tissues or fluids derived from testing on a patient or withdrawn in some clinic. The database contains attributes such as anonymized patient id, specimen type, biobank locations, and other anonymized medical history information such as prior testing or patient demographics.
- Find all specimens that are instances of blood or bone marrow, derived from a woman younger than 25 years old, who has not tested positive for cancer (and if there has been a negative test result for cancer, include that information), and the specimen is located in the state of Illinois, Indiana, Michigan, or Ohio.
- NOTE 1: This is largely from ChatGPT, generated 2023-03-25
- NOTE 2: I am kinda hoping you rate this as a Kata-3. Nudge.
```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dct: <http://purl.org/dc/terms/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX ncit: <http://ncicb.nci.nih.gov/xml/owl/EVS/Thesaurus.owl#>
PREFIX obo: <http://purl.obolibrary.org/obo/>

SELECT ?specimen ?age ?test_result ?state
WHERE {
  ?specimen a ncit:C7057, ?type .
  FILTER (?type IN (obo:OBI_0000299, obo:OBI_0002026))  # blood or bone marrow
  ?specimen dct:subject ?subject .
  ?subject ncit:C16960 ?gender ;  # gender
           ncit:C25197 ?age ;     # age
           ncit:C32701 ?state .   # state
  FILTER (?gender = ncit:C16576)   # female
  FILTER (?age < 25^^xsd:integer)  # younger than 25
  OPTIONAL {
    ?subject ncit:C25194 ?test .  # cancer test
    ?test ncit:C25195 ?test_result ;
          ncit:C25196 ncit:C25205 .  # negative result
  }
  FILTER (!bound(?test_result))   # no positive cancer test
  FILTER (?state IN (ncit:C34404, ncit:C34403, ncit:C34410, ncit:C34405))  # IL, IN, MI, or OH
}
```

**Problem 4.4 ... or Kata-3?** 
- The FBI wants to put several people on a watchlist if they have performed some suspicious activity. Suppose that they wanted to put anyone on the watchlist who fit the following conditions: the person owns a phone that was present at the US Capital Building on January 6, 2022. There are some ways to connect the device to the building by finding such cases where the mobile device has certain GPS coordinates and communicating with the cell tower such information. This would be too hasty, except that they want to prioritize those who have had a criminal record (bearer of 'FormerCriminalRole') and not a political figure nor a journalist ('PoliticalOfficeRole' and 'JournalistRole', respectively).
- Assert with CONSTRUCT that such persons bear a FBIWatchlistMemberRole
- NOTE: Feel free to tell me that this is a Kata-3?
```
PREFIX cco: <http://www.ontologyrepository.com/CommonCoreOntologies/>
PREFIX ro: <http://purl.obolibrary.org/obo/ro.owl>
PREFIX ontopol: <https://politicalontology.org/schema/ontopol.ttl>
PREFIX telephont: <https://telephonyontology.org/schema/telephont/>

CONSTRUCT REDUCED {
	?Person ro:bearer_of ?FBIWatchlistMemberRole . 
	}
WHERE {
	?MobileDevice ro:participates_in ?CellTowerCommunicationProcess;
		telephont:has_phone_model ?PhoneModel ;
		telephont:has_latitude_value ?LatitudeValue ; 
		telephont:has_longitude_value ?LongitudeValue ;
	?Person telephont:has_phone_carrier ?PhoneCarrier ;
		telephont:has_phone_model ?PhoneModel .
	?CellTowerCommunicationProcess ro:has_part ?TowerCommunicationProcessSegment .
	?TowerCommunicationProcessSegment cco:occurs_on "2021-01-06T14:30:00+06:00" .
	FILTER ( regex( ?LatitudeValue, "^38.888"|"^38.889"|"^38.89") ) .
	FILTER ( regex( ?LongitudeValue, "^-77.008"|"^-77.009"|"^-77.01") ) .
	FILTER EXISTS (?Person cco:described_by ?FormerCriminalRole )
	FILTER NOT EXISTS (?Person ro:bearer_of ?PoliticalOfficeRole ) .
	FILTER NOT EXISTS (?Person ro:bearer_of ?JournalistRole ) .
}

```

..._phew_!
