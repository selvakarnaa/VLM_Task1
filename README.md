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


index

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>⚙️ Turbine Multimodal VLM Quality Verification Hub</title>
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background-color: #f4f6f9; 
            margin: 0; 
            padding: 20px; 
            color: #333;
        }
        .container { 
            max-width: 1200px; 
            margin: 0 margin: auto; 
            background: white; 
            padding: 30px; 
            border-radius: 8px; 
            box-shadow: 0 4px 12px rgba(0,0,0,0.1); 
        }
        h1 { 
            color: #1e3a8a; 
            border-bottom: 2px solid #e2e8f0; 
            padding-bottom: 10px; 
            margin-top: 0;
        }
        .upload-section { 
            background: #f8fafc; 
            border: 2px dashed #cbd5e1; 
            padding: 25px; 
            border-radius: 6px; 
            margin-bottom: 30px; 
            text-align: left; 
        }
        .prompt-input { 
            width: 100%; 
            padding: 12px; 
            border: 1px solid #cbd5e1; 
            border-radius: 4px; 
            font-size: 14px; 
            margin-top: 5px;
            margin-bottom: 15px; 
            box-sizing: border-box; 
            font-family: inherit;
        }
        .file-input {
            margin-top: 5px;
            margin-bottom: 20px;
            display: block;
        }
        .btn { 
            background: #2563eb; 
            color: white; 
            padding: 12px 25px; 
            border: none; 
            border-radius: 4px; 
            cursor: pointer; 
            font-size: 16px; 
            font-weight: bold; 
            transition: background 0.2s ease;
        }
        .btn:hover { 
            background: #1d4ed8; 
        }
        .grid { 
            display: grid; 
            grid-template-columns: 1fr 1fr; 
            gap: 30px; 
            margin-bottom: 30px; 
        }
        .card { 
            border: 1px solid #e2e8f0; 
            padding: 20px; 
            border-radius: 6px; 
            background: #fff; 
            text-align: center; 
        }
        .card h3 {
            color: #334155;
            margin-top: 0;
            margin-bottom: 15px;
            border-bottom: 1px solid #f1f5f9;
            padding-bottom: 8px;
        }
        .card img { 
            max-width: 100%; 
            max-height: 350px; 
            border-radius: 4px; 
            object-fit: contain; 
            box-shadow: 0 2px 6px rgba(0,0,0,0.05);
        }
        table { 
            width: 100%; 
            border-collapse: collapse; 
            margin-top: 20px; 
            box-shadow: 0 2px 8px rgba(0,0,0,0.04);
        }
        th, td { 
            padding: 14px 18px; 
            text-align: left; 
            border-bottom: 1px solid #e2e8f0; 
            font-size: 14px; 
            line-height: 1.5;
        }
        th { 
            background-color: #0f172a; 
            color: white; 
            font-weight: 600; 
            letter-spacing: 0.5px;
        }
        tr:hover { 
            background-color: #f8fafc; 
        }
        .success-banner { 
            background-color: #dcfce7; 
            color: #15803d; 
            padding: 15px; 
            border-radius: 4px; 
            font-weight: bold; 
            margin-bottom: 20px; 
            border: 1px solid #bbf7d0; 
        }
        .alert-banner { 
            background-color: #fee2e2; 
            color: #b91c1c; 
            padding: 15px; 
            border-radius: 4px; 
            font-weight: bold; 
            margin-bottom: 20px; 
            border: 1px solid #fca5a5; 
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>⚙️ Turbine Multimodal VLM Quality Verification Hub</h1>
        <p>Drop your shop-floor picture asset below and enter an evaluation instruction statement pass to validate the component live.</p>
        
        <!-- Upload Form Interface Block -->
        <div class="upload-section">
            <form action="/inspect" method="POST" enctype="multipart/form-data">
                <label style="font-weight: bold;">🔧 User Verification Instruction Prompt:</label>
                <input type="text" name="prompt" class="prompt-input" value="Validate the image and give the report" required>
                
                <label style="font-weight: bold;">📷 Choose Test Image File Component:</label>
                <input type="file" name="file" accept=".png, .jpg, .jpeg" class="file-input" required>
                
                <button type="submit" class="btn">🔍 Run Hybrid RAG Inspection Analysis</button>
            </form>
        </div>

        <!-- Runtime Connection or Matrix Error Banners -->
        {% if error_msg %}
            <div class="alert-banner">❌ {{ error_msg }}</div>
        {% endif %}

        <!-- Interactive Evaluation Metrics Output Dashboard -->
        {% if result_logged %}
            <div class="success-banner">✅ Conformance Record Logged! New row successfully appended to Excel ledger: '{{ output_excel }}'</div>
            
            <div class="grid">
                <!-- Left Panel View: Live Input Asset -->
                <div class="card">
                    <h3>📸 Uploaded Shop-Floor Test Asset Image</h3>
                    <img src="data:image/png;base64,{{ test_img_b64 }}" alt="Uploaded Image">
                </div>
                
                <!-- Right Panel View: Local Database Lookalike Standard Baseline Match -->
                <div class="card">
                    <h3>📚 Closest Visual Database Standard Match</h3>
                    {% if ref_img_b64 %}
                        <img src="data:image/png;base64,{{ ref_img_b64 }}" alt="Matched Database Image">
                    {% else %}
                        <p style="color: #64748b; font-style: italic; padding: 50px 0;">Reference visual match logged but file missing from hard drive paths.</p>
                    {% endif %}
                </div>
            </div>

            <!-- Spreadsheet Presentation Record Table Layout Mapping Rows -->
            <h2>📊 Generated Excel Matrix Row Record Presentation</h2>
            <table>
                <thead>
                    <tr>
                        <th style="width: 25%;">Excel Target Column Name Header</th>
                        <th style="width: 75%;">Dynamic Value Log Entry Output</th>
                    </tr>
                </thead>
                <tbody>
                    {% for field, val in metrics.items() %}
                    <tr>
                        <td style="font-weight: bold; color: #1e3a8a;">{{ field }}</td>
                        <td>{{ val if val else "—" }}</td>
                    </tr>
                    {% endfor %}
                </tbody>
            </table>
        {% endif %}
    </div>
</body>
</html>

