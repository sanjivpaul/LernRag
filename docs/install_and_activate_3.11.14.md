This line proves it beyond doubt:

```
(.venv) $ python --version
Python 3.14.0
```

So your virtualenv is:

```
.venv/bin/python  → Python 3.14
```

---

## What happened

When you originally ran something like:

```bash
uv venv .venv
```

`uv` used your **default system Python**, which is:

```
/opt/homebrew/bin/python3 → Python 3.14
```

So the venv is _permanently bound_ to 3.14.

You cannot “downgrade” a venv.
It must be **deleted and recreated**.

---

## The exact fix (copy–paste)

### 1. Delete the broken venv

```bash
cd ~/Developer/Projects/project_45_RAG/RAG
rm -rf .venv
```

---

### 2. Create a real 3.11 venv

```bash
uv venv --python 3.11 .venv
source .venv/bin/activate
```

---

### 3. Verify (must say 3.11)

```bash
python --version
which python
```

### 4. Fix jupyter specific (for 3.11.14)

```bash
uv pip install ipykernel
python -m ipykernel install --user --name rag311 --display-name "RAG (Python 3.11)"
```

You should see:

```
Python 3.11.x
.../.venv/bin/python
```

If it says 3.14 again → stop, something is wrong.

---

### 4. Reinstall your packages

```bash
uv pip install chromadb sentence-transformers numpy scikit-learn
uv pip install "pydantic<2"
```

---

## Why your previous attempt didn’t work

This is the subtle but critical part:

> A virtualenv **hard-links the interpreter at creation time**.

So even if Python 3.11 exists on your machine, your current `.venv` is **physically built around 3.14** forever.

No package install can change that.

---

## Optional but smart: pin Python version

So this never happens again:

```bash
uv python pin 3.11
```

Now any future:

```bash
uv venv
```

will default to 3.11.

---

## Final mental model

Right now you had:

```
Python 3.14 system
→ created venv
→ venv is Python 3.14 forever
→ Chroma explodes
```

After fix:

```
Python 3.11 system
→ create venv
→ venv is Python 3.11
→ Chroma works
```

---

### One-line truth

Your venv is not “misconfigured” — it is **correctly configured to the wrong Python**.
Delete it and recreate with `uv venv --python 3.11`.
