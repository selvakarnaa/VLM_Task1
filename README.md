# 1. Create a clean, isolated virtual environment folder named 'rvnv'
python -m venv rvnv

# 2. Activate the virtual environment workspace (Windows)
rvnv\Scripts\activate
Flask==3.0.3
chromadb==0.5.3
python-docx==1.1.2
openpyxl==3.1.5
pandas==2.2.2
pillow==10.3.0
opencv-python-headless==4.9.0.80
numpy==1.26.4
requests==2.32.3
pip install -r requirements.txt --trusted-host pypi.org --trusted-host files.pythonhosted.org
# 1. Extract .docx data chunks and compute pixel math maps completely offline
python build_multimodal_db.py

# 2. Launch your web server interface asset gateway channel 
python run_vlm_flask_app.py
