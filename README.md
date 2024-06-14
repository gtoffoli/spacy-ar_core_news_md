# Unofficial Arabic pipeline for the *spaCy* framework

## About

Basic information on this release can be found in the README of the package https://github.com/gtoffoli/spacy-cameltokenizer, which constitutes a prerequisite, together with the *CAMeL Tools* library by *CAMeL-Lab* (https://github.com/CAMeL-Lab/camel_tools).

Further information on the the problems encountered and on the motivations of some choices can be found in the *discussion* https://github.com/explosion/spaCy/discussions/7146

## Installation

I assume that you work in a Python "virtual environment" (*venv*), where possibly you already installed *spaCy*.
You also need a local *git* directory to clone 2 packages from *GitHub*:

```
git clone https://github.com/gtoffoli/spacy-cameltokenizer.git
git clone https://github.com/gtoffoli/spacy-ar_core_news_md.git
```
Then, in the *site-packages* directory of your *venv*, create 2 *symbolic links*:
- `cameltokenizer`, linking to the `cameltokenizer` sub-directory of the local `spacy-cameltokenizer` repository;
- `ar_core_news_md`, linking to the `ar_core_news_md` sub-directory of the local `spacy-ar_core_news_md` repository.

Finally, install *spaCy* (if needed) and the *CAMeL Tools* library:

```
pip install spacy
pip install camel-tools
```
## spaCy customization

Replace 2 modules in the `spacy/lang/ar` subdirectory of the `spaCy` directory in *site-packages*, taking the new ones from the `spacy_lang_ar_custom` sub-directory of the local `spacy-ar_core_news_md` repository:

- `__init__.py`
- `punctuation.py`

## Pipeline initialization

In a *settings* module of your applications (in my case it is the `settings.py` of a *Django* app), put the following code:
```
	import spacy
	from cameltokenizer import tokenizer

	ar = spacy.load('ar_core_news_md')
	cameltokenizer = tokenizer.CamelTokenizer(ar.vocab)

	@Language.component("cameltokenizer")
	def tokenizer_extra_step(doc):
		return cameltokenizer(doc)

	ar.add_pipe("cameltokenizer", name="cameltokenizer", first=True)
```