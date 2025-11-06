# NFCorpus Medical Search Engine

A Django web-based search engine implementing the BM25+LambdaMart method for medical information retrieval capabilities. This project provides intelligent search functionality over the NFCorpus dataset, which contains medical documents from NutritionFacts.org.

## Features

- **BM25 Algorithm**: Implements BM25 ranking function for document retrieval
- **LambdaMart Learning-to-Rank**: Advanced machine learning model for improving search relevance
- **Medical Document Collection**: Searches through medical documents from the NFCorpus dataset
- **Web Interface**: Clean Django-based web interface for search functionality
- **Performance Metrics**: Real-time search performance tracking

## Dataset

This project uses the NFCorpus (NutritionFacts Corpus) dataset, a full-text learning to rank dataset for medical information retrieval. The dataset contains documents from NutritionFacts.org with relevance judgments.

### Citation
If you use this corpus, please cite:
```
@inproceedings{boteva16full,
    title="A Full-Text Learning to Rank Dataset for Medical Information Retrieval",
    author = "Vera Boteva and Demian Gholipour and Artem Sokolov and Stefan Riezler",
    booktitle = "Proceedings of the European Conference on Information Retrieval ({ECIR})",
    location = "Padova, Italy",
    publisher = "Springer"
    year = 2016,
}
```

## Prerequisites

- Python 3.8+
- pip (Python package manager)
- Virtual environment (recommended)

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/ferdifdi/nfcorpus_medical_search_engine.git
cd nfcorpus_medical_search_engine
```

### 2. Create and Activate Virtual Environment

**Windows (PowerShell):**
```powershell
# Create virtual environment
python -m venv venv

# Activate virtual environment
.\venv\Scripts\Activate.ps1
# If you get an error about execution policies, run:
# Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

**Windows (Command Prompt):**
```cmd
# Create virtual environment
python -m venv venv

# Activate virtual environment
venv\Scripts\activate.bat
```

**macOS/Linux:**
```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate
```

**Verify activation:** You should see `(venv)` at the beginning of your command prompt.

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

**Note:** Make sure you have activated the virtual environment before installing dependencies. This ensures all packages are installed in the isolated environment.

### 4. Database Setup
```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. Create Superuser (Optional)
```bash
python manage.py createsuperuser
```

## Running the Application

### Development Server

**Important:** Always activate your virtual environment before running Django commands!

```bash
# Windows PowerShell
.\venv\Scripts\Activate.ps1

# Windows Command Prompt
venv\Scripts\activate.bat

# macOS/Linux
source venv/bin/activate

# Then run the server
python manage.py runserver
```

The application will be available at `http://localhost:8000`

### Production Deployment

#### Using Docker
```bash
# Build the image
docker build -t nfcorpus-search .

# Run the container
docker run -p 8000:8000 nfcorpus-search
```

#### Using Heroku
```bash
# Install Heroku CLI and login
heroku login

# Create Heroku app
heroku create your-app-name

# Push to Heroku
git push heroku master

# Run migrations
heroku run python manage.py migrate
```

## Project Structure

```
nfcorpus_medical_search_engine/
├── home/                           # Main Django app
│   ├── collection/                 # Document collection (organized by folders)
│   ├── data/                      # Training data
│   ├── index/                     # Search index files
│   ├── nfcorpus/                  # NFCorpus dataset
│   ├── result/                    # Model results
│   ├── static/                    # Static files (CSS, JS)
│   ├── templates/                 # HTML templates
│   ├── bsbi.py                    # BSBI indexing implementation
│   ├── compression.py             # Index compression utilities
│   ├── letor.py                   # Learning to Rank implementation
│   ├── myModel.py                 # Main model logic
│   └── views.py                   # Django views
├── my_app/                        # Django project settings
├── requirements.txt               # Python dependencies
├── manage.py                      # Django management script
├── Dockerfile                     # Docker configuration
├── Procfile                       # Heroku deployment config
└── README.md                      # This file
```

## Usage

1. **Start the Application**: Run the development server using `python manage.py runserver`

2. **Access the Search Interface**: Navigate to `http://localhost:8000` in your web browser

3. **Perform Searches**: Enter medical-related queries in the search box. The system will:
   - Process your query using the BM25 algorithm
   - Apply LambdaMart ranking for improved relevance
   - Return ranked results from the medical document collection

4. **View Results**: Click on any search result to view the full document content

## Technical Implementation

### Search Algorithm
- **BM25**: Probabilistic ranking function for document retrieval
- **LambdaMart**: Gradient boosting algorithm for learning-to-rank
- **Vector Space Model**: Document representation using TF-IDF and topic modeling

### Key Components
- **BSBI (Block-Sort-Based Indexing)**: Efficient indexing for large document collections
- **Compression**: VBE (Variable Byte Encoding) for index compression
- **Feature Engineering**: Combines multiple features including:
  - TF-IDF vectors
  - Topic model representations (LDA with 200 topics)
  - Jaccard similarity
  - Cosine distance

## Configuration

### Environment Variables
- `DATABASE_URL`: PostgreSQL connection string for production
- `DEBUG`: Set to `False` in production
- `SECRET_KEY`: Django secret key

### Database
- **Development**: SQLite3 (default)
- **Production**: PostgreSQL (configured via `DATABASE_URL`)

## Dependencies

Key Python packages:
- **Django 4.1.4**: Web framework
- **scikit-learn**: Machine learning algorithms
- **lightgbm**: LambdaMart implementation
- **gensim**: Topic modeling
- **nltk**: Natural language processing
- **pandas**: Data manipulation
- **numpy**: Numerical computing

See `requirements.txt` for complete dependency list.

## Performance

The search engine provides:
- Real-time query processing
- Sub-second response times for most queries
- Scalable indexing for large document collections
- Memory-efficient index compression

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Create a Pull Request

## License

This project is available for academic use. For commercial use of the NFCorpus data, please refer to the [NutritionFacts.org terms of service](http://nutritionfacts.org/terms-of-service) and contact Dr. Michael Greger directly.

## Troubleshooting

### Common Issues

1. **ModuleNotFoundError: No module named 'django'**
   - **Cause**: Virtual environment is not activated or Django is not installed
   - **Solution**: 
     ```bash
     # Activate virtual environment first
     # Windows PowerShell:
     .\venv\Scripts\Activate.ps1
     
     # Windows Command Prompt:
     venv\Scripts\activate.bat
     
     # macOS/Linux:
     source venv/bin/activate
     
     # Verify activation (you should see (venv) in prompt)
     # Then install requirements if not already done:
     pip install -r requirements.txt
     ```

2. **PowerShell Execution Policy Error**
   - **Error**: `cannot be loaded because running scripts is disabled on this system`
   - **Solution**:
     ```powershell
     Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
     ```

3. **Import Errors**: Ensure all dependencies are installed with `pip install -r requirements.txt` (inside activated venv)

4. **Database Issues**: Run migrations with `python manage.py migrate`

5. **Static Files**: Collect static files with `python manage.py collectstatic`

6. **Index Files Missing**: Ensure the `home/index/` directory contains the pre-built index files

7. **Memory Issues**: For large datasets, consider increasing system memory or using index compression

### Support

For issues and questions:
- Check existing GitHub issues
- Create a new issue with detailed error information
- Include system information and steps to reproduce

## Acknowledgments

- NFCorpus dataset creators: Vera Boteva, Demian Gholipour, Artem Sokolov, Stefan Riezler
- NutritionFacts.org for providing the medical document collection
- Django community for the excellent web framework