#.Project intro : influenced by "corrective rag langgraph " agentic rag 
Tip : incase you need to uncommit run : git reset --soft HEAD~1 or git restore --staged . or git stash pop 
Tip : to kill any open server ports : # force kill one liner : lsof -ti tcp:8000 | xargs -r kill -9
# Tech stack AI BAsed bot , RAG based , Conversactional RAG  , scrapping data from amazonn and flipcard, store data in database vecort , astradb to store vectors db , we can use aws openserch alternate 
# Kubernetes , AWS infra Cloudformation, github actions 

ETL<> ELT : Extract transform load , Extract load transform 
FIRECRAWLER or Zepre  to extract data from website like html  
we will use bs4 - BeautifulSoup for scrapping html from webpages 
# techstack - vector db datastrack model response grader , langchain , MCP server , duckduckgo web sccraper and product search from flipcart 
# MCP server tools - retrival as tool, websearch from duckduckgo and product info from flipcart (retrieval.py as a tool)
# -----------------------------------------------
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
# --------------------UI---------------------------

15. to launch steamlit ui : streamlit run scrapper_ui.py
16. run python prod_assistant/retriever/retrieval.py
17. run application : uvicorn prod_assistant.router.main:app --reload --port 8000
18. ask any question in chat , it should be able to anser based on your astra db , if you wish to scrape more data run scraper_ui froom another terminal so that info can be uploaded to astradb simultinessly
# -----------------------------------------------
# langgraph session 40 (node , MCP , edge(simple , conditional), state , agentic ai, multi-agentic-ai, and Agentic -RAG workflow ,observability or validation and memory of agent)
19. agentic ai framework are - > Langchain, autogen, crewai,n8n , openai kernel , A2A, hugging face small agent ,pydata agno 
20. No code framework - Langhgraph , autogen, n8n  
21. langgraph best one 
22. FAISS Documentation - to refer index mechanism , retriever techniques - vecttor embedding techniques like ANN(approximate nearesr neighbour) KNN, HNSN,IVF etc 
# -----------------------------------------------
23. session 42 : MCP  
# ----------- MCP Server folder under prod assistant ---------
# run prod_search_server first then client from mcp_server folder
# AI documentation product resource https://www.xenodocs.com/chat

# --- test MCP 
24. run python prod_assistant/mcp_servers/product_search_server.py
25 run : python prod_assistant/mcp_servers/client.py

# Kill server if its already running , first get PID 
lsof -i tcp:8000 
# then note pid and run below :kill <PID>
# force kill : kill -9 <PID>
# force kill one liner : lsof -ti tcp:8000 | xargs -r kill -9

# ----------- test ----------
scrapper : python prod_assistant/etl/data_scrapper.py
scrapper UI : streamlit run scrapper_ui.py
try using upload to astradb button 
from cmd
python prod_assistant/etl/data_ingestion.py
python prod_assistant/retriever/retrieval.py
# ---------LLM and provider config util/model_loader----
 
 provider_key = os.getenv("LLM_PROVIDER", "openai") # Changed default to openai/google/groq

# --------Test MCP ---------
MCP server running up : python prod_assistant/mcp_servers/product_search_server.py
test MCP server by calling Client : python prod_assistant/mcp_servers/client.py

# ------ Test agentic workflow with MCP & Webserach 
python prod_assistant/workflow/normal_generation_workflow.py
python prod_assistant/workflow/agentic_rag_workflow.py
python prod_assistant/workflow/agentic_workflow_with_mcp.py
python prod_assistant/workflow/agentic_workflow_with_mcp_websearch.py

# -----Running fast API and app UI with chat option ------
command : uvicorn prod_assistant.router.main.app --reload --port 8000 or 
command : uvicorn prod_assistant.router.main:app --reload --port 8002

if any error e.g. async etc , run with python 3.11 version (delete current venv , create new venv with python 3.11 version and install requirement.txt again)
# --------- Docker ---------
Check docker installed : docker -v
make sure to run docker from your desktop 
check docker images : docker images
to delete any image : docker rmi <image id >
Check any container running : docker ps

# ----- Create Docker Image - ------
Tip : make sure you have docker file 
Create image : docker build -t prod-assistant .

# ----- Run Docker Container -------
docker run -d -p 8000:8000 --name <container_custom_name> <created_image_name >
docker run -d -p 8000:8000 --name product-assistant prod-assistant

# ------check image running --------
browser launch : http://localhost:8000/

# ----- Create Cloud Formation Code for INFRA using AWS -------
# ---- GITHUB Secrets storage or you can store on aws secret as well

go to github account > your repo > settings>secrets>action>new repository secrets 
e.g.  ecomm-prod-assistant/settings/secrets/actions/new
add below secrets from your .env file 
GOOGLE_API_KEY
OPENAI_API_KEY
ASTRA_DB_API_ENDPOINT
ASTRA_DB_APPLICATION_TOKEN
ASTRA_DB_KEYSPACE

# aws details from below steps

AWS_ACCESS_KEY_ID= aws user access key e.g.
AWS_SECRET_ACCESS_KEY= aws user secret key e.g. 
AWS_REGION= us-east-1

# below name from infra/eks-with-ecr.yaml file 

ECR_REPOSITORY=product-assistant
EKS_CLUSTER_NAME=product-assistant-cluster-latest
ECR_REGISTRY = 299059642276.dkr.ecr.us-east-1.amazonaws.com
     ---get it from aws console >ECR >Create repo , copy id of r3egistory 
     ---make sure deployment.yaml had same id and region e.g. container.image: 29905964.dkr.ecr.us-east-1.amazonaws.com/product-assistant:latest

# -----AWS secrets 
IAM > Create user > add username > attach policies > select AdministratorAccess > next > Click Create User 
Select username > Security credentials > Create access Key > Command line interface (cli) >select check box > next >Create Access Key 
Copy user secret access key and token 

# ---- GitHub Infra script trigger -------
go to your git hub 
repo > actions > click on provision Infra (EKS+ECR ) from left panel > click on run workflow  from right pane
your infra will be created
application deplyment will trigger 

# --- Check your infra ------
login to aws console > cloudformation> stacks 
-- based on your region selected , all servercies and repo will be created

# ---- install aws cli on your system 
from cmd run below
Check cli instlled : aws --version
run : aws configure
enter aws acces key id (Iam user)
enter aws secret access key 
enter region us-east-1
output format none - hit enter 

TIP : whenever you check in code , it autoatically trigger deployment but failed , since we did not run provision infra (EKS+ECR )

# -- Provision INFRA - GIT Create INFRA ---
go to your repo > actions > click on Provision Infra (EKS+ECR) to run it . 
-- it make take 10-20 minutes

# -- how to check / verify -----
Aws console > cloudformation > stack (in your specified reasion )
# ---- once deployment is completed - get all details from cli 

from cammand prompt check below 
aws
aws configure
aws eks update-kubeconfig --name product-assistant-cluster-latest --region us-east-1
kubectl get nodes
# to get external - ip address where your application is running , same details you can from from AWS console >ec2 > load balancer >click on id > check DNS name 
kubectl get svc -o wide 

# get details of applications 
kubectl describe svc product-assistant-service
# get pod details 
kubectl get pods -o wide
# to get any logs like error / logs /debug logs 
kubectl logs <name from previous commnad pod e.g. product-assistant-8739234243-82347> 

kubectl exec -it product-assistant-776b47db47-tpqjb --curl http://localhost:8000
doskey /history
