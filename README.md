# Fingerprint Based Localization
## Part 1: Initial Data Processing
1. Import the training and testing csv files in Google Colaboratory
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=40mm,keepaspectratio]{import_csv.png}
        \end{figure}%*}
2. Get all unique AP addresses in the dataset and store them as a list.
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=40mm,keepaspectratio]{get_all_ap.png}
        \end{figure}%*}
3. Impute the dataset by assigning -120 dBm for the missing signal strengths by the following method:
    1. Create a new dataframe with the column names initialized with the AP addresses derived in the previous step.
    2. Copy all the existing signal strength values for the APs already present in the input dataframe for each row, and then assign -120 to all the remaining cells.
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=70mm,keepaspectratio]{impute.png}
        \end{figure}%*}

## Part 2: Localization
1. Find Euclidean distance for each row in test dataframe with the entire training dataframe to find the k-Nearest Neighbours for each data point in the test dataframe, using the formula:
    \[distance = \sqrt{\sum_{i=1}^{M} {(RSS_{A,i}-RSS_{B,i})}^2}\]
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=17mm,keepaspectratio]{euclidean.png}
        \end{figure}%*}
2. Filter the k-Nearest data points based on the Euclidean distance calculated as above.
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=6mm,keepaspectratio]{knn.png}
        \end{figure}%*}
3. Estimate the latitude and longitude values of the test data points by taking the average of the latitudes and longitudes of k-Nearest Neighbours.
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=40mm,keepaspectratio]{estimate.png}
        \end{figure}%*}
4. Calculate localization errors by using the formula:
        \[dlon = lon2 - lon1 \]
        \[dlat = lat2 - lat1 \]
\[a = (sin(dlat/2))^2 + cos(lat1) * cos(lat2) * (sin(dlon/2))^2 \]
\[c = 2 * atan2( sqrt(a), sqrt(1-a) ) \]
\[d = R * c  \] where R (radius of the Earth) = 3961 (in miles) \& 6373 (in km)

## Result
1. In miles:
    1. When k = 3 Nearest Neighbours are considered
        1. Median error: 0.05412130633831588
        2. 67 percentile error: 0.07257826825888594
        3. 90 percentile error: 0.1349382128040697
    2. When k = 4 Nearest Neighbours are considered:
        1. Median error: 0.06651225517358414
        2. 67 percentile error: 0.08843083221850023
        3. 90 percentile error: 0.15952889777751433
    3. When k = 5 Nearest Neighbours are considered:
        1. Median error: 0.07828930635986234
        2. 67 percentile error: 0.10608027759309598
        3. 90 percentile error: 0.17633717991532963
2. In km:
    1. When k = 3 Nearest Neighbours are considered:
        1. Median error: 0.08707777967535651
        2. 67 percentile error: 0.11677387114715478
        3. 90 percentile error: 0.21710710179256154
    2. When k = 4 Nearest Neighbours are considered:
        1. Median error: 0.10701403742015948
        2. 67 percentile error: 0.14227965001981874
        3. 90 percentile error: 0.2566719680727339
    3. When k = 5 Nearest Neighbours are considered:
        1. Median error: 0.1259625724391322
        2. 67 percentile error: 0.17067649813198704
        3. 90 percentile error: 0.28371543741489413
