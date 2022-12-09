from functools import reduce
import random


def getRedundantBitsCount(bitsLen):
    # By using the formula 2 ^ r >= m + r + 1
    for i in range(bitsLen):
        if (2**i >= bitsLen + i + 1):
            return i


def placeRedundantBits(data, nosRedBits):
    # Redundancy bits are placed at the positions set as '0'
    # which correspond to the power of 2.
    j = 0
    k = 1
    dataLen = len(data)
    res = ''

    # If position is power of 2 then insert '0'
    # Else append the data bit
    for i in range(1, dataLen + nosRedBits + 1):
        if(i == 2**j):
            res = res + '0'
            j += 1
        else:
            res = res + data[-1 * k]  # getting databits from LSB
            k += 1

    # The result is reversed since positions are
    # counted backwards. (m + r+1 ... 1)
    return res[::-1]


def calcParityBits(data, nosRedBits):
    dataLen = len(data)

    # For finding rth parity bit, iterate over
    # 0 to r - 1
    for i in range(nosRedBits):
        val = 0  # 0 for even parity, 1 for odd parity
        for j in range(1, dataLen + 1):

            # If position has 1 in ith significant
            # then Bitwise X-OR the array value
            # to find parity bit value.
            if(j & (2**i) == (2**i)):
                val = val ^ int(data[-1 * j])
                # -1 * j is given since array is reversed

        # String Concatenation
        # (0 to n - 2^r) + parity bit + (n - 2^r + 1 to n)
        data = data[:dataLen-(2**i)] + str(val) + data[dataLen-(2**i)+1:]

    return data


def detectError(data, nosRedBits):
    n = len(data)
    res = 0

    # Calculate parity bits again
    for i in range(nosRedBits):
        val = 0
        for j in range(1, n + 1):
            if(j & (2**i) == (2**i)):
                val = val ^ int(data[-1 * j])

        # Create a binary no by appending
        # parity bits together.

        res = res + val*(10**i)

    # Convert binary to decimal
    return int(str(res), 2)


def main():

    data = input("Enter Data: ")
    dataLen = len(data)
    # How many Reduant bits required
    nosRedBits = getRedundantBitsCount(dataLen)

    # Determine the positions of Redundant Bits
    hammingCodeBits = placeRedundantBits(data, nosRedBits)

    # Determine the parity bits
    hammingCodeBits = calcParityBits(hammingCodeBits, nosRedBits)

    # Data to be transferred
    print("Hamming Code Generated : " + hammingCodeBits)

    # Stimulate error in transmission by changing
    # a bit value.

    arr = list(hammingCodeBits)  # copying List
    changedIndex = random.randint(0, len(arr)-2)
    arr[changedIndex] = '1' if arr[changedIndex] == '0' else '0'
    arr = "".join(arr)
    print("Error Data is " + arr)
    # detect the position of error bit
    correction = detectError(arr, nosRedBits)
    # compare detected error with what we actually created
    print("The position of error is " + str(correction) +
          "\t Changed Bit index : " + str(len(arr) - changedIndex))

    correction = len(arr) - correction
    # correct and print
    newValue = '1' if arr[correction] == '0' else '0'
    print("Corrected : " + arr[:correction-1] + newValue + arr[correction+1:])


if __name__ == "__main__":
    main()
