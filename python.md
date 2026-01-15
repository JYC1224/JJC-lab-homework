height = 5

for i in range(1, height + 1):
    print('*' * i)


height = 5

print("--- 正三角形 (金字塔) ---")
for i in range(1, height + 1):
    print(' ' * (height - i) + '* ' * i)
