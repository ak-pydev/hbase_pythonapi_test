HBase Demo: Courses

This repository contains a small Jupyter notebook demo showing how to interact with Apache HBase from Python using HappyBase. It creates a simple `courses` table, inserts sample data, performs basic CRUD operations, and converts the results into a pandas DataFrame.

Tools Used
- Jupyter Notebook (VS Code or Jupyter)
- Python libraries: `happybase`, `pandas`
- Apache HBase (local)
- HBase Thrift server on `localhost:9090`
- HBase Shell (optional, to verify data)

What the Notebook Does
The notebook `hbase_demo.ipynb`:
- Installs dependencies with `%pip` and imports `happybase` and `pandas`.
- Connects to HBase via Thrift on `localhost:9090`.
- Creates the table `courses` if it does not exist with two column families:
  - `info` (e.g., `info:name`, `info:credits`)
  - `students` (e.g., `students:1`, `students:2`, ...)
- Defines helper functions:
  - `put_course(...)` to insert a course row with name, credits, and students
  - `print_all(...)` to scan and pretty-print rows
  - `to_df(...)` to scan and return a `pandas.DataFrame`
- Inserts example rows, scans the table, updates one row, deletes another, and shows the data as a DataFrame.

HBase Schema
- Table: `courses`
- Row key: course ID (e.g., `STA210`)
- Column families and qualifiers:
  - `info:name`, `info:credits`
  - `students:1`, `students:2`, ... (one qualifier per student)

Prerequisites
- Java installed and HBase set up locally.
- HBase running with the Thrift server enabled on port `9090` (binary protocol used by HappyBase).

Example commands (adjust `HBASE_HOME` to your installation):

```bash
export HBASE_HOME=${HBASE_HOME:-/path/to/hbase}
"$HBASE_HOME/bin/start-hbase.sh"
"$HBASE_HOME/bin/hbase" thrift start -p 9090
```

If you prefer, you can verify the table in HBase Shell:

```bash
"$HBASE_HOME/bin/hbase" shell
> list
> describe 'courses'
> scan 'courses'
```

Running the Notebook
- Open `hbase_demo.ipynb` in VS Code or Jupyter.
- Ensure HBase and the Thrift server are running.
- Run cells from top to bottom. The first cell installs `happybase` and `pandas` in the active kernel.

If you need to install dependencies manually for your Python environment:

```bash
pip install happybase pandas
```

Troubleshooting
- Connection refused / transport errors: ensure the Thrift server is running on `localhost:9090`.
- Table already exists: the notebook creates the table only if it’s missing.
- No rows returned: re-run the insert cell (`put_course(...)` calls) and scan again.

---
This demo is for educational purposes in DSC-421 to illustrate basic HBase access patterns from Python.