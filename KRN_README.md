1. from terminal run uv init ecomm-prod-assistant
2. make sure you have 3.10 in .python-version and pyproject.toml requires-python = ">=3.10"
3. open vscode, open folder project you just created 
4. run  command: uv python install cpython-3.11.13-macos-aarch64-none
5. create virtual env by running command: uv venv venv --python cpython-3.10.19-macos-aarch64-none  
6. activate venv: source venv/bin/activate
7. uv pip install -r requirements.txt
8. install liveserver extention to view html file 
9. pip install steamlit 
10. To show application UI to fetch realtime data related to rpoducts from external shopping sites : streamlit run scrapper_ui.py 
11. 



