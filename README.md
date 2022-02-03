# VoteEntry
VoteEntry function in Election Run Off Program

public static List<string> VoteEntry (int voters, int ranks, List<string> candidateList)
{
            voters = 2;
            ranks = 2;
            var votesEntered = new List<string>();
            // create and fill selection list for ranking choices with user inputs
            var selection = new List<string>();
            while (true)
            {
                for (var k = 0; k < candidateList.Count; k++)//display cadidates with numbers
                    Console.WriteLine("{0}) {1}", k + 1, candidateList[k]);
                Console.WriteLine("Please choose {0} candidate(s) represented by numbers", ranks);
                Console.WriteLine("(e.g. For 1.John Smith, you enter 1).");
                Console.WriteLine("Please press 'enter key' once to enter the next vote.");
                Console.WriteLine();
                var i = 0;
                while (i < voters)//i represents each voter
                {
                    var choiceRanks = new List<string>();//this list exists to get rid of duplicates from the same voter

                    var j = 0;
                    while (j < ranks)
                    {
                        Console.Write("voter{0}: ", i + 1);
                        var temp = Console.ReadLine();

                        if (!String.IsNullOrWhiteSpace(temp))
                        {
                            //while (d < temp.Length) breaks down string to char to make sure each char is valid
                            var d = 0;
                            while (d < temp.Length)
                            {
                                //skip any white space from the user input
                                if (Char.IsWhiteSpace(temp[d]))
                                    d++;
                                //any valid char will be stored to a list (choiceRank)
                                else if (Char.IsDigit(temp[d]))
                                    if (!choiceRanks.Contains(temp[d].ToString()))
                                    {
                                        choiceRanks.Add(temp[d].ToString());
                                        d++;
                                    }
                                    else
                                        d++;
                                else// cover non letter inputs
                                    d++;
                            }
                            //put each char into one string to cover any number bigger than 9
                            var joined = String.Join("", choiceRanks.ToArray());
                            if (!String.IsNullOrWhiteSpace(joined))
                            {
                                // forloop to check if the value is within the candidates numbers
                                for (var k = 0; k < candidateList.Count; k++)
                                    if (Convert.ToInt32(joined) == k + 1)
                                    {
                                        //the final step to add each value to a list 
                                        if (!selection.Contains(joined))
                                        {
                                            selection.Add(joined);
                                            //clear the list so we can store the next value
                                            choiceRanks.Clear();
                                            j++;
                                            //if statement to see if have enough values(j) stored for each voter(i)
                                            if (selection.Count == ranks)
                                            {
                                                //move the values from voter(i) to votesEntered => use it for vote counting 
                                                foreach (var choice in selection)
                                                    votesEntered.Add(choice);
                                                //clear selection list, so it can store next voter(i)'s values 
                                                selection.Clear();
                                                //increment i to end while (j < ranks)
                                                i++;
                                            }
                                        }
                                    }
                                    else
                                        choiceRanks.Clear();
                            }
                            else//covers non digit entry
                            {
                                choiceRanks.Clear();
                            }
                        }
                    }
                }
                // when all the values are validated and stored in to votesEntered list you can be out of the top while(true) loop
                break;
            }
            return votesEntered;
        }        
