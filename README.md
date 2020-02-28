\documentclass[11pt]{article}
\usepackage{amsmath,amssymb,xspace,epsfig}

\textwidth 6.5in \textheight 9.05in \oddsidemargin 0.0in
\evensidemargin 0.0in \topmargin -0.55in
\addtolength{\textwidth}{2.5mm} \addtolength{\columnsep}{2mm}

%\long\def\soln#1{#1}
\long\def\soln#1{}

\title{CSE570: Wireless And Mobile Networks - Spring 2020}
\author{Aiswarya Lakshmi Renganathan $($SBU ID: 112688118$)$\thanks{%
Department of Computer Science, Stony Brook University, Stony Brook,
NY 11794, USA, {\tt arenganathan@cs.stonybrook.edu}. } }
\usepackage{tabto}
\usepackage{amsmath}
\usepackage{breqn}
\begin{document}
\maketitle
\section*{Homework 2}
\textbf{Methodology}
\par \textbf{Part 1: Initial Data Processing}
\begin{enumerate}
    \item Import the training and testing csv files in Google Colaboratory
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=40mm,keepaspectratio]{import_csv.png}
        \end{figure}%*}
    \item Get all unique AP addresses in the dataset and store them as a list.
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=40mm,keepaspectratio]{get_all_ap.png}
        \end{figure}%*}
    \item Impute the dataset by assigning -120 dBm for the missing signal strengths by the following method:
    \begin{enumerate}
        \item Create a new dataframe with the column names initialized with the AP addresses derived in the previous step.
        \item Copy all the existing signal strength values for the APs already present in the input dataframe for each row, and then assign -120 to all the remaining cells.
    \end{enumerate}
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=70mm,keepaspectratio]{impute.png}
        \end{figure}%*}
\end{enumerate}
\par \textbf{Part 2: Localization}
\begin{enumerate}
    \item Find Euclidean distance for each row in test dataframe with the entire training dataframe to find the k-Nearest Neighbours for each data point in the test dataframe, using the formula:
    \[distance = \sqrt{\sum_{i=1}^{M} {(RSS_{A,i}-RSS_{B,i})}^2}\]
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=17mm,keepaspectratio]{euclidean.png}
        \end{figure}%*}
    \item Filter the k-Nearest data points based on the Euclidean distance calculated as above.
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=6mm,keepaspectratio]{knn.png}
        \end{figure}%*}
    \item Estimate the latitude and longitude values of the test data points by taking the average of the latitudes and longitudes of k-Nearest Neighbours.
        \begin{figure}[h]%*}[htp]
            \centering
            \includegraphics[width=\textwidth,height=40mm,keepaspectratio]{estimate.png}
        \end{figure}%*}
    \item Calculate localization errors by using the formula:
        \[dlon = lon2 - lon1 \]
        \[dlat = lat2 - lat1 \]
\[a = (sin(dlat/2))^2 + cos(lat1) * cos(lat2) * (sin(dlon/2))^2 \]
\[c = 2 * atan2( sqrt(a), sqrt(1-a) ) \]
\[d = R * c  \] where R (radius of the Earth) = 3961 (in miles) \& 6373 (in km)
\end{enumerate}
\textbf{Result}
\begin{enumerate}
\item In miles:
\begin{enumerate} 
\item When k = 3 Nearest Neighbours are considered:
    \begin{enumerate} 
\item Median error: 0.05412130633831588
\item 67 percentile error: 0.07257826825888594
\item 90 percentile error: 0.1349382128040697
    \end{enumerate}
\item When k = 4 Nearest Neighbours are considered:
    \begin{enumerate} 
\item Median error: 0.06651225517358414
\item 67 percentile error: 0.08843083221850023
\item 90 percentile error: 0.15952889777751433
    \end{enumerate}
\item When k = 5 Nearest Neighbours are considered:
\begin{enumerate} 
\item Median error: 0.07828930635986234
\item 67 percentile error: 0.10608027759309598
\item 90 percentile error: 0.17633717991532963
     \end{enumerate}
\end{enumerate}
\item In km:
\begin{enumerate} 
\item When k = 3 Nearest Neighbours are considered:
    \begin{enumerate} 
\item Median error: 0.08707777967535651
\item 67 percentile error: 0.11677387114715478
\item 90 percentile error: 0.21710710179256154
    \end{enumerate}
\item When k = 4 Nearest Neighbours are considered:
    \begin{enumerate} 
\item Median error: 0.10701403742015948
\item 67 percentile error: 0.14227965001981874
\item 90 percentile error: 0.2566719680727339
    \end{enumerate}
\item When k = 5 Nearest Neighbours are considered:
\begin{enumerate} 
\item  Median error: 0.1259625724391322
\item 67 percentile error: 0.17067649813198704
\item 90 percentile error: 0.28371543741489413
     \end{enumerate}
\end{enumerate}
\end{enumerate}

\end{document}
