
import csv    # Imported csv for csv writing 

def read_genes(file_path):
    f = open(file_path)
    gene_dict = {}
    keys = []     
    values = []
    # for each line in the said file if it has anything other than A,G,C,T,whitespace,\n it has to be a key,
    # and if it doesn't its a gene and we append it into values
    for item in f.readlines():
        if item.strip('AGTC >\n'):
            keys.append(item.strip().strip('>'))
        else:
            values.append(item.strip())
    for i in range(len(keys)):
        gene_dict[keys[i]] = values[i]
    gene_dict.keys()


    return gene_dict


def get_fragments(gene_dict: dict, frag_len=50):
    frag_dict = {}
    fragKeys = []
    fragValues = []

    keys = list(gene_dict.keys())            # We put values and keys into a list for easier manipulation
    values = list(gene_dict.values())

    for i in range(len(values)):
        # Value Variables
        tempFragValue = ''
        value = values[i]
        tempValue = ''
        fragCount = len(values[i]) // frag_len

        # Key Variables
        tempFragKey = ''
        key: str = keys[i]
        keyChr = key.split('|')[0]
        keyIndex = int(key.split('|')[1].split('-')[0])

        for i in range(fragCount): 
            # Value Calculations
            tempFragValue = value[0:frag_len]
            tempValue = value[frag_len:len(value)]
            value = tempValue
            fragValues.append(tempFragValue)

            # Key Calculations
            tempFragKey = keyChr + '|'
            tempFragKey += str(keyIndex) + '-'
            keyIndex += frag_len
            tempFragKey += str(keyIndex)     #keyIndex becaomes last index in each iteration
            fragKeys.append(tempFragKey)

    for i in range(len(fragKeys)):
        frag_dict[fragKeys[i]] = fragValues[i]

    return frag_dict


def filter_frags(frag_dict, threshold=0.7):
    def get_similarity(s1, s2):   
        similarity = 0
        total = len(s1)
        for i in range(len(s1)):
            if(s1[i] == None or s2[i] == None):
                # do literally nothing if they exceed index for at least one of them
                passOn = True
            elif s1[i] == s2[i]:
                similarity += 1

        return similarity/total

    dissimilarFragDict = {}
    keys = list(frag_dict.keys())
    values = list(frag_dict.values())
    
    for i in range(1,len(values)):
        if get_similarity(values[i-1],values[i]) >= threshold:  # checking if the similarity level is higher than 0.7
            values.pop(i)
            keys.pop(i)

    for i in range(len(keys)):
        dissimilarFragDict.update({keys[i]:values[i]})
    
    return dissimilarFragDict


def get_sentences(dissimilar_frag_dict: dict):
    def generate_kmers(seq: str, k):
        if k > len(seq):    # If somehow k is larger than lenght, it pops up an error message and ends the generatekmers
            print('Unexpected Error')
            return
        sentence = ''
        # # For mathematical conveniences k -=1
        # k -= 1
        for i in range(len(seq)-k + 1):
            sentence += seq[i:i+k] + ' '
        sentence = sentence.rstrip()
        return sentence

    sentencesDict = {}
    keys = list(dissimilar_frag_dict.keys())
    values = list(dissimilar_frag_dict.values())
    for i in range(len(values)):
        values[i] = generate_kmers(values[i], 4)    #Each list value becomes its own 4 kmer sentence 
    for i in range(len(values)):
        sentencesDict[keys[i]] = values[i]
    return sentencesDict


def clean_dict(sentences_dict: dict):
    def clean_sentence(sentence: str):
        sentenceList = sentence.split(' ')
        uniqueList = []
        for item in sentenceList:
            if not item in uniqueList:
                uniqueList.append(item)      # Appending unique items into another list 
        newSentence = ''
        for item in uniqueList:
            newSentence += item+' '

        return newSentence.rstrip()

    keys = list(sentences_dict.keys())
    values = list(sentences_dict.values())
    cleanDict = {}
    for i in range(len(values)):
        values[i] = clean_sentence(values[i])
    for i in range(len(values)):
        cleanDict[keys[i]] = values[i]
    return cleanDict


def write_genes(file_path,clean_sentences_dict):
    f = open(f"{file_path}", "w", newline="")
    keys = list(clean_sentences_dict.keys())
    values = list(clean_sentences_dict.values())

    fieldnames = ["fragment_id","sentence","sentence_lenght","number_of_words"]    # writing header lines 
    writer = csv.writer(f)
    writer.writerow(fieldnames)
    for i in range(len(keys)):
        no = 1
        for j in values[i]: # counting the spaces between the keys so to find the amount of sentences
            if j == " ":
                no+=1
            
        writer.writerow([f"{keys[i]}",f"{values[i]}",f"{len(values[i])}",f"{no}"]) 
    f.close()
    return 


def main():

    # 1) read genes from input.txt 
    read_genes("C:/Users/mehme/Desktop/input.txt")
    #    print the number of genes -> Expected output: 115
    geneDict = read_genes("C:/Users/mehme/Desktop/input.txt")
    print(len(geneDict))

    # 2) get fragments for genes read
    get_fragments(geneDict)
    #    print the number of fragments -> Expected output: 1293
    fragDict = get_fragments(geneDict)
    print(len(fragDict))

    # 3) filter out similar fragments
    filter_frags(fragDict)
    #    print the number of dissimilar fragments -> Expected output: 1286
    filtered_dict = filter_frags(fragDict)
    print(len(filtered_dict))

    # 4) get sentences for dissimilar fragments
    get_sentences(filtered_dict)
    #    print the number of words in a sentence -> Expected output: 46
    sentenceDict = get_sentences(filtered_dict)
    # 5) remove duplicate words in sentences
    clean_dict(sentenceDict)
    #    print the total number of words removed -> Expected output: 8194
    cleanDict = clean_dict(sentenceDict)
    # 6) write sentences into output.csv
    write_genes("C:/Users/mehme/Desktop/output.csv",cleanDict)





main()
