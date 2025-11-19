Nice — here are two ready-to-run **Flask** versions you can copy into files and run with `python`.

Quick: install Flask if you don't have it:

```bash
pip install flask
```

---

# 1) Even numbers app — save as `even_app.py`

```python
from flask import Flask, request, render_template_string

app = Flask(__name__)

HTML = """
<!doctype html>
<html>
  <body>
    <h3>Generate N Even Numbers</h3>
    <form method="post">
      Enter N: <input type="number" name="n" min="0" value="{{n}}">
      <input type="submit" value="Generate">
    </form>
    <p>{{ result }}</p>
  </body>
</html>
"""

@app.route("/", methods=["GET", "POST"])
def main():
    result = ""
    n_val = ""
    if request.method == "POST":
        n_val = request.form.get("n", "") or "0"
        try:
            n = int(n_val)
            if n < 0:
                result = "Please enter a non-negative integer."
            else:
                evens = [str(2 * i) for i in range(1, n + 1)]
                result = ", ".join(evens) if evens else "[]"
        except ValueError:
            result = "Invalid input. Please enter an integer."
    return render_template_string(HTML, result=result, n=n_val)

if __name__ == "__main__":
    app.run(port=8080, debug=True)
```

Run:

```bash
python even_app.py
# open http://localhost:8080/
```

---

# 2) Matrix multiplication app — save as `matrix_app.py`

```python
from flask import Flask, request, render_template_string
from markupsafe import escape   # <-- FIXED IMPORT

app = Flask(__name__)

HTML = """
<!doctype html>
<html>
  <body>
    <h3>Matrix Multiplication</h3>
    <form method="post">
      <p>Matrix A (rows separated by ';', values by ',')</p>
      <input type="text" name="A" size="60" value="{{A}}">
      <p>Matrix B (rows separated by ';', values by ',')</p>
      <input type="text" name="B" size="60" value="{{B}}">
      <br><br>
      <input type="submit" value="Multiply">
    </form>

    <div style="margin-top:10px; font-family:monospace;">
      {{ result|safe }}
    </div>
  </body>
</html>
"""

def parse_matrix(text):
    text = (text or "").strip()
    if not text:
        return []
    rows = []
    for row_text in text.split(';'):
        row_text = row_text.strip()
        if not row_text:
            continue
        parts = [p.strip() for p in row_text.split(',') if p.strip()]
        row = [int(p) for p in parts]
        rows.append(row)
    if not rows:
        return []
    # Ensure consistent columns
    cols = len(rows[0])
    if any(len(r) != cols for r in rows):
        raise ValueError("Rows have unequal lengths.")
    return rows

def multiply(A, B):
    if not A or not B:
        return []
    rowsA, colsA = len(A), len(A[0])
    rowsB, colsB = len(B), len(B[0])
    if colsA != rowsB:
        raise ValueError(f"Cannot multiply: columns of A ({colsA}) != rows of B ({rowsB}).")
    C = [[0] * colsB for _ in range(rowsA)]
    for i in range(rowsA):
        for j in range(colsB):
            C[i][j] = sum(A[i][k] * B[k][j] for k in range(colsA))
    return C

def matrix_to_html(M):
    if not M:
        return "Result: []"
    rows_html = ""
    for r in M:
        cells = "".join(f"<td>{escape(v)}</td>" for v in r)
        rows_html += f"<tr>{cells}</tr>"
    return f"Result:<br><table border='1' cellpadding='6' style='border-collapse:collapse;'>{rows_html}</table>"

@app.route("/", methods=["GET", "POST"])
def main():
    result_html = ""
    A_text = request.form.get("A", "")
    B_text = request.form.get("B", "")

    if request.method == "POST":
        try:
            A = parse_matrix(A_text)
            B = parse_matrix(B_text)
            C = multiply(A, B)
            result_html = matrix_to_html(C)
        except Exception as e:
            result_html = f"<b>Error:</b> {escape(str(e))}"

    return render_template_string(
        HTML,
        result=result_html,
        A=escape(A_text),
        B=escape(B_text),
    )

if __name__ == "__main__":
    app.run(port=8080, debug=True)
```

Run:

```bash
python matrix_app.py
# open http://localhost:8080/
```

---

If you want I can:

* Combine both into a single Flask app with two routes (`/even` and `/matrix`),
* Add nicer textareas for easier multi-line matrix entry, or
* Provide a NumPy-backed faster version (if you plan to multiply large matrices).

Which would you like next?
