# Bank campaign

A bank wants to know, before the call, whether a customer will subscribe to a term deposit. The data is the UCI Bank Marketing set in `data/bank-additional-full.csv`: about 41,000 calls from May 2008 to November 2010. The whole lesson is `lesson.ipynb`.

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Open the notebook and select the **(.venv)** kernel. Run the cells from top to bottom. Each cell is one small step. `duration` is left out of the model because the call length is unknown before the call. The split follows file order: 70% train, 15% test, 15% live. A chronological slice of the training period selects the decision threshold for F1; the test and live periods are not used to select it.

After the Docker cells have written `serve/`:

```powershell
docker build -t bank-campaign ./serve
New-Item -ItemType Directory -Force logs
docker run --rm -p 8000:8000 -v "${PWD}/logs:/logs" bank-campaign
```

Leave the container running and run the remaining cells. They send the live period to the service and compare it with the training distribution. Stop the container with Ctrl+C.
