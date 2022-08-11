# ice-tracking
Ice Tracking

Question: 
Since the air-ice boundary is generally stronger than ice-rock and easier to estimate, it makes sense to first estimate the air-ice boundary, 
and then estimate the ice-rock boundary but require that it be at least (for example) 10 pixels below the air-ice boundary in each column (i.e., require that rx ≥ ax + 10 for all x ∈ [1, m]).

Solution:
The basic problem is to keep the track of ice height in polar regions to get the estimate for increasing temperature.
We have to detect the boundary of air-ice and ice-rpck from the radar echogram.

In the solution we consider few assumptions:
1. The air-ice boundary is always above the ice-rock boundary by a significant margin (say, 10 pixels) i.e. vertical distance
    between air-ice and ice-rock pixel is atleast 10.
2. The two boundaries span the entire width of the image i.e. pixel for boundary exists in every column of the image grid.
3. we assume that each boundary is relatively “smooth” — that is, a boundary’s row in next column has the high probabiltity of being.

FIRST PART: 
Using Bayes Net:
In this technique, I simply took out the maximum from each column(edge matrxi) and considered it as the most probable pixel to be part of
the boundary.
Then, after estimating the air-ice boundary, and considering the assumption that there is at least 10 pixel vertical distance
between air-ice and ice-rock boundary, i took the edge pixel with maxiumum value from the array[ airIceBoundary[i]: ] i.e. from column number 10 down of air-ice boundary.

Yellow Line shows the boundary output for bayes net.


SECOND PART:
Using Viterbi:
For viterbi algorithm, i need few probabilities into consideration:
1. Initial Probability - I considered the pixel with maximum edge value as my initial point and set its probability to 1
2. Transition Probabilities - Here I considered the assumption, the boundary is smooth. So, in the range of 15, i assumed probabilites for the pixels in the same row
                            for next column. The farthest the pixel, the lesser the probability.
3. Emission Probabilites - I considered my edge values as my emission probabilites( as they are my observed values). I have normalised them, to fall in the range of 
                            0-1, as to be considered as probabilites.

-After having all the required values, i have calculated my boundary indexes for air-ice boundary by viterbi algorithm.

-After getting the air-ice boundary indexes, To calculate the ice-rock boundary, I considered the assumption of minimum vertical distance being 10.
Appling the same process(viterbi) to pixels down 10 to air-ice boundary, to detect the ice-rock boundary.

Blue Line shows the boundary output for viterbi algorithm.

THIRD PART:
Considering Human Coordinates:
This is an modification to second part, wherein we consider providen human Coordinates as the one pixel on the boundary, and start our algorithm from the point.

For viterbi algorithm, i need few probabilities into consideration:
1. Initial Probability - I considered the pixel with maximum edge value as my initial point and set its probability to 1
2. Transition Probabilities - Here I considered the assumption, the boundary is smooth. So, in the range of 15, i assumed probabilites for the pixels in the same row
                            for next column. The farthest the pixel, the lesser the probability.
3. Emission Probabilites - I considered my edge values as my emission probabilites( as they are my observed values). I have normalised them, to fall in the range of 
                            0-1, as to be considered as probabilites.

Here, we are providen value for any column , so we need to find the values for (0-column) and (column+1- totalColumns)

After having all the required values, i have calculated my boundary indexes for air-ice boundary by viterbi algorithm(forward and backward).
Same process applied for detecting the ice-rock boundary.

Red Line shows the boundary output for viterbi algorithm.


#Run 
python3 ./iceTracking.py input_file.jpg airice_row_coord airice_col_coord icerock_row_coord icerock_col_coord
