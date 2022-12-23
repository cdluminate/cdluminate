main:
	pdflatex cv.tex
	-evince cv.pdf
	-$(MAKE) clean

quiet:
	pdflatex cv.tex
	-$(MAKE) clean

clean:
	rm *.log *.out *.aux
