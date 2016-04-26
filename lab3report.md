# Lab 3 -- Dynamic Programming

* Jeremy Castillo
* jcc4428

## Optimal Substructure

The problem has optimal substructure because the subproblems of it are part of the optimal solution. The case I used to understand this is the problem with 2 courses and 1 hour. We are able to initialize the (0, h) and (i, 0) squares in the table. (0, h) corresponds to the grade you gets for spending each section of time on the first course. (i, 0) corresponds to the grade one gets for spending no time on all courses. This leaves the table with one empty square which is solved by finding the max grade between:
    
    grade when working 1 hour on second course + grade when working 0 hour on first course

    and
    
    grade when working 0 hour on second course + grade when working 1 hour on first course.

The max of these two numbers is the max total possible grade one could get, and it is in the (n-1, H) location of the table. This extends to any combinations of n courses and H hours as the subproblem solution in each square contributes to the optimal solution of the entire problem.

## Recurrence Relation

The recurrence relation I used is based on the argument of the optimal substructure of the problem. A 2D array is made with 0 <= i <= n, where n is the number of course and 0 <= j <= H where H is the total hours. Let's call it T[n][H+1]. Also gf(i, k) is the grade one gets corresponding to a specific course i and hour k.

    for(k < H)
        T[i, j] = max ( gf(i, k) + T[i-1][j-k] ) for all k.

## Algorithm for iteratively computing optimal solution

    create gradeTable[numClasses][totalHours+1]
    create timeTable[numClasses][totalHours+1]

    for k to totalHours 
        gradeTable[0][k] = grade for class id 0 and hour worked k;
        timeTable[0][k] = hour spent k
    for k to numClasses
        gradeTable[k][0] = grade for class k when no time spend
        timeTable[k][0] = 0

     // find max grade
        for each class k
            for each hour j
                int maxGrade = 0;
                int maxHour = 0;

                // this loop finds the maximum total grade for the current subproblem
                // it does that by finding all the combinations of time put towards the current class k
                // and adding it to the solution to the [k-1] (exclude current class) [j-h] (include time from total hour j)
                // it also keeps track the maximum hour (time corresponding to the max grade) another table
                for each hour h
                    if( j >= h ) 
                        int tmpGrade = grade for k class and hour h 
                        tmpGrade += solution to subproblem without this course and current time spent on it
                        if (tmpGrade > maxGrade) 
                            maxGrade = tmpGrade;
                            maxHour = h;
                        endif
                    endif
                
                gradeTable[k][j] = maxGrade;
                timeTable[k][j] = maxHour;
            
        

## Algorithm for recreating opitmal solution
 
        // map the max grade to the courses
         create computeHours[] which is size of numClasses
        

        // these indices correspond to the amount of time that is used one the last course in the optimal solution
        // this is bc in gradeTable the optimal solution is in precisely these indices
        int c = numClasses -1;
        int h = totalHours;

        // this loop maps the class id to it's time in the optimal solution
        // this works because the grade and time tables each contain the 
        // optimal solutions to subproblems that contribute to the solution of // the original problem
        while(c > 0) {
            computeHours[c] = timeTable[c][h];
            h -= computeHours[c];
            c--;
        }
        computeHours[c] = h;

        return computeHours;

## Testing Approach

For testing I used three grade functions to ensure the functionality of my implementation. The first two were provided (Custom Grade and Square Grade) and helped show the size of input my implementation handles. It handled the smaller sizes that which tested the mapping of hours to grade function, and the finding of the total grade with both a small and large amount of time. Finally I implemented my own grade function which tested my implementation when the use of zero for a grade was used solely when no time was put into a course. These tests covered the range of inputs one should test when implementing a method: the negative infinity case (where input is small or incorrect), the general cases (the assumed cases the method should handle alwasy) and the positive infinity cases (where input becomes large).

