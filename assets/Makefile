main:
	pdflatex cv.tex
	pdflatex cv.tex
	-papers cv.pdf
	-open -a Preview cv.pdf
	-$(MAKE) clean

quiet:
	pdflatex cv.tex
	pdflatex cv.tex
	-$(MAKE) clean

clean:
	rm -f *.log *.out *.aux
