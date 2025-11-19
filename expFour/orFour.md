Here is a **minimal WebApp2 code** exactly like your even-numbers example, but performing **matrix multiplication** instead.

This version takes two matrices as comma-separated rows, multiplies them, and prints the result.

---

### ✅ **Matrix Multiplication WebApp2 Code**

```python
import webapp2

HTML = """
<html>
  <body>
    <h3>Matrix Multiplication</h3>
    <form method="post">
      <p>Enter Matrix A (rows separated by ';', values by ',')</p>
      <input type="text" name="A" placeholder="e.g. 1,2;3,4">

      <p>Enter Matrix B (rows separated by ';', values by ',')</p>
      <input type="text" name="B" placeholder="e.g. 5,6;7,8">

      <br><br>
      <input type="submit" value="Multiply">
    </form>

    <p>{result}</p>
  </body>
</html>
"""

def parse_matrix(text):
    # "1,2;3,4" → [[1,2],[3,4]]
    return [list(map(int, row.split(","))) for row in text.split(";")]

def multiply(A, B):
    rowsA, colsA = len(A), len(A[0])
    rowsB, colsB = len(B), len(B[0])

    # Basic validation
    if colsA != rowsB:
        return None

    # Initialize result matrix
    C = [[0] * colsB for _ in range(rowsA)]

    # Multiply
    for i in range(rowsA):
        for j in range(colsB):
            for k in range(colsA):
                C[i][j] += A[i][k] * B[k][j]

    return C

def matrix_to_string(M):
    return "<br>".join(str(row) for row in M)


class Main(webapp2.RequestHandler):
    def get(self):
        self.response.write(HTML.format(result=""))

    def post(self):
        A_text = self.request.get("A")
        B_text = self.request.get("B")

        try:
            A = parse_matrix(A_text)
            B = parse_matrix(B_text)
            C = multiply(A, B)

            if C is None:
                result = "Error: Columns of A must equal rows of B!"
            else:
                result = "Result:<br>" + matrix_to_string(C)

        except:
            result = "Invalid matrix format!"

        self.response.write(HTML.format(result=result))


app = webapp2.WSGIApplication([('/', Main)], debug=True)
```

---

### ✔ Example Input

Matrix A:

```
1,2;3,4
```

Matrix B:

```
5,6;7,8
```

Output:

```
[19, 22]
[43, 50]
```

