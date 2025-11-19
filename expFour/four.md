4th ->
```
import webapp2

HTML = """
<html>
  <body>
    <h3>Generate N Even Numbers</h3>
    <form method="post">
      Enter N: <input type="number" name="n">
      <input type="submit" value="Generate">
    </form>
    <p>{result}</p>
  </body>
</html>
"""

class Main(webapp2.RequestHandler):
    def get(self):
        self.response.write(HTML.format(result=""))

    def post(self):
        n = int(self.request.get('n', '0'))
        evens = [str(2*i) for i in range(1, n+1)]
        result = ", ".join(evens)
        self.response.write(HTML.format(result=result))

app = webapp2.WSGIApplication([('/', Main)], debug=True)
```