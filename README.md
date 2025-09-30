# DAA-practical-4
maximum sum of subarray


#include <stdio.h>
struct Result {
    int low, high, sum;
};

struct Result findMaxCrossSum(int resource[], int low, int mid, int high) {
    int leftSum= -10000000;
    int sum = 0;
    int maxLeft = mid;

    for (int i = mid; i >= low; i--) {
        sum += resource[i];
        if (sum > leftSum) {
            leftSum = sum;
            maxLeft = i;
        }
    }

    int rightSum = -1000000;
    sum = 0;
    int maxRight = mid + 1;

    for (int j = mid + 1; j <= high; j++) {
        sum += resource[j];
        if (sum > rightSum) {
            rightSum = sum;
            maxRight = j;
        }
    }

    struct Result res;
    res.low = maxLeft;
    res.high = maxRight;
    res.sum = leftSum + rightSum;
    return res;
}

struct Result maxResult(struct Result a, struct Result b, struct Result c) {
    if (a.sum >= b.sum && a.sum >= c.sum)
        return a;
    else if (b.sum >= a.sum && b.sum >= c.sum)
        return b;
    else
        return c;
}

struct Result maxSubArray(int resource[], int low, int high) {
    if (low == high) {
        struct Result res;
        res.low = low;
        res.high = high;
        res.sum = resource[low];
        return res;
    }

    int mid = (low + high) / 2;
    struct Result leftRes = maxSubArray(resource, low, mid);
    struct Result rightRes = maxSubArray(resource, mid + 1, high);
    struct Result crossRes = findMaxCrossSum(resource, low, mid, high);

    return maxResult(leftRes, rightRes, crossRes);
}

int main() {
    int resource[] = {5, 1, -2, 8, 3, -4};
    int n = sizeof(resource) / sizeof(resource[0]);
    struct Result result = maxSubArray(resource, 0, n - 1);
    printf(" %d  %d %d\n",result.low, result.high, result.sum);
    return 0;
}

