#.Project intro : AI BAsed bot , RAG based , Conversactional RAG  , scrapping data from amazonn and flipcard, store data in database vecort , astradb to store vectors db , we can use aws openserch alternate 
ETL<> ELT : Extract transform load , Extract load transform 
FIRECRAWLER or Zepre  to extract data from website like html  
we will use bs4 - BeautifulSoup for scrapping html from webpages 


1. from terminal run uv init ecomm-prod-assistant
2. make sure you have 3.10 in .python-version and pyproject.toml requires-python = ">=3.10"
3. open vscode, open folder project you just created 
4. run  command: uv python install cpython-3.11.13-macos-aarch64-none
5. create virtual env by running command: uv venv venv --python cpython-3.10.19-macos-aarch64-none  
6. activate venv: source venv/bin/activate
7. uv pip install -r requirements.txt
8. install liveserver vscode extention to view html file 
9. pip install steamlit 
10. To show application UI to fetch realtime data related to rpoducts from external shopping sites : streamlit run scrapper_ui.py 
11. instade of createing venv we can also use "uv add -r requirement.txt" but we will use traditional way 
12. For accessing the DataStax, for astradb from casandra here is a link: https://accounts.datastax.com/session-service/v1/login
121. once logged in > crete DB > name it > copy ASTRA_DB_API_ENDPOINT and generate token and copy it to .env variable 
122. Go to data explorer and note keyspace copy that to .env file 
13. Vectordb Comparison: https://superlinked.com/vector-db-comparison
14. Once you log in to the DataStax Vector page, you will get the following page


15. to launch steamlit ui : streamlit run scrapper_ui.py
16. run python prod_assistant/retriever/retrieval.py





