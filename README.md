# Online Petitioning Through Data Exploration and What We Found There: A Dataset of Petitions from Avaaz.org

Notebook and dataset from our ICWSM 2018 dataset paper. 
The code of the choropleth maps is based on https://github.com/yaph/d3-geomap.
When using this dataset, please use the following citation:

> Aragón P., Sáez-Trumper D., Redi M., Hale S. A., Gómez V., Kaltenbrunner A. (2018) Online Petitioning Through Data Exploration and What We Found There: A Dataset of Petitions from Avaaz.org ICWSM-18 - 12th International AAAI Conference on Web and Social Media, Palo Alto, California, United States.

## Authors
Pablo Aragón<sup>1,2</sup>, Diego Sáez-Trumper<sup>1</sup>, Miriam Redi<sup>3</sup>, Scott A. Hale<sup>4</sup>, Vicenç Gómez<sup>1</sup>, Andreas Kaltenbrunner<sup>1</sup>

1. Universitat Pompeu Fabra
2. Eurecat, Centre Tecnològic de Catalunya
3. Wikimedia Foundation
4. Oxford Internet Institute, University of Oxford


## Crawling process
To generate the dataset of petitions from Avaaz.org, we implemented a web crawler based on the incremental nature of their numerical ids. First, for a given petition id, a script sent a request to the AJAX endpoint of Avaaz.org and retrieved the corresponding URL. Then, with the petition URL, another script fetched and parsed the HTML to extract and store the corresponding metadata. The crawling process was done in August 2016 and the petition ids ranged from 1 to 382979 (which was the latest petition at that time). After excluding deleted pages, we obtained a dataset of 366,214 petitions.

It is important to highlight two issues taken into account when the crawler was designed. First, the [machine-readable robots.txt file on Avaaz.org](https://secure.avaaz.org/robots.txt) does not specify any restrictions. Second, every page fetched by the crawler specified a _Creative Commons Attribution 3.0 Unported License_ in the footnote. Therefore, our dataset is released under the same terms.


## Files
The petitions are available at `petitions.csv` (zipped). Besides the id and URL, each petition contains the following fields:

- _title_ (string): Title of the petition (limited to 100 characters). Following the [official guidelines about how to write a petition title](https://secure.avaaz.org/en/petition/how_to_write_a_petition_title/), many of them include the person, organization and/or location it addresses.

- _description_ (string): Description of the petition. 

- _author_ (string): Name of the user who authored the petition. To preserve anonymity, _Avaaz.org_ includes only the given name and the first initial of the family name.

- _date_ (timestamp): Date when the petition was published, from December 2011 to August 2016. Because dates were originally found in different languages including different writing systems (Latin, Arabic, Cyrillic, Kana, Hebrew, and Greek), we standardized them as _yyyy-MM-dd_.

- _country_name_ (string): Name of the country of origin of the author. Users are able to customize this field in their profiles. Otherwise, the value is obtained by _Avaaz.org_ directly from the user's IP address.
- _country_code_ (string): As well as_date_, country names were originally found in different languages and writing systems, e.g., Turkey was also found as  تركي_, Türkiye, Türkei, Turchia, Turquie, Turquía, and Турция. Therefore, we standardized countries using [ISO 3166-1 alpha-3 codes](https://unstats.un.org/unsd/methodology/m49).
- _sign_ (integer): Number of signatures at the time the petition was crawled. 

- _target_ (integer): Number of signatures set as a goal by the platform/creator (this value may change as petitions receive signatures).

- _ratio_ (float): Ratio between the two proceeding fields.

- _facebook_count_ (integer): Number of shares on Facebook.

- _twitter_count_ (integer): Number of shares on Twitter.

- _whatsapp_count_ (integer): Number of shares on WhatsApp.

- _email_count_ (integer): Number of shares via email.

- _lang_code_ (string): Language detected on the concatenation of title and description using a [plugin for Apache Nutch project](https://wiki.apache.org/nutch/LanguageIdentifier) (based on n-grams of over 50 languages). The resulting language is standardized with a [ISO-639 2-letter code](http://www.loc.gov/standards/iso639-2/php/code_list.php).

- _lang_prob_ (float): Probability of success given by the language detection plugin.

- _people_ (multivalued string): The names of people found within the concatenation of the title and description fields using the [Stanford Named Entity Recognizer (NER)](https://nlp.stanford.edu/software/CRF-NER.html). Results are only provided when the detected language was English, Spanish or German (available languages).

- _organizations_ (multivalued string): The names of organizations found using the NER, as done for _people_. 

- _locations_ (multivalued string): Locations found using the NER, as done for _people_. 

- _miscellany_ (multivalued string): Other entities found using the NER, as done for _people_. 

This repository also contains the Jupyter notebook of the data exploration process (`exploration.ipynb`) and three folders (`csv`, `img`, `maps`) with input and output files for the notebook.

## Acknowledgment
This work is supported by the Spanish Ministry of Economy and Competitiveness under the María de Maeztu Units of Excellence Programme (MDM-2015-0502), and the mobility grant offered by Societat Econòmica Barcelonesa d'Amics del País.
