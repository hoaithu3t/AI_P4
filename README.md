# AI_P4

Bài 1:
Xác suất con ma xuất hiện ở mỗi vị trí được tính bằng xác suất xuất hiện của ma khi biết khoảng cách từ con Pacman đến con ma nhân với xác suất của con ma xuất hiện ở ô đó ban đầu khi không có điều kiện gì

```python
allPossible[p] = emissionModel[trueDistance] * self.beliefs[p]
```

Bài 2:
Xác suất con ma xuất hiện ở vị trí kế tiếp sẽ bằng xác suất ở vị trí hiện tại nhân với xác suất con ma sẽ xuất hiện ở các vị trí tiếp theo:

```python
newBeliefs[newPos] += probability * self.beliefs[oldPos]
```


Bài 3:
Tìm ra vị trí có khả năng xuất hiện nhất của mỗi ma và khoảng cách từ pacman đến những vị trí đó:


    mostLikeLyGhostPositions = []
            for index in range(len(livingGhostPositionDistributions)):
                highestProb, mostProbPosition = 0, None
                for position, prob in livingGhostPositionDistributions[index].items():
                    if prob > highestProb:
                        highestProb, mostProbPosition = prob, position
                mostLikeLyGhostPositions.append((mostProbPosition, self.distancer.getDistance(mostProbPosition, pacmanPosition)))
                                                      
 Sau đó tìm ra vị trí của ma gần nhất và khoảng cách từ pacman đến đó:
 
    leastDistance = float('inf')
      closestPosition = None
      for ghostPosition, distance in mostLikelyGhostPositions:
        if distance < leastDistance:
          leastDistance = distance
          closestPosition = ghostPosition
                                                          
 Cuối cùng chọn ra action tốt nhất:
 
    bestNewDistance = float('inf')
          bestAction = []
          for action in legal:
              successorPosition = Actions.getSuccessor(pacmanPosition, action)
              newDistance = self.distancer.getDistance(closestPosition, successorPosition)
              if newDistance < bestNewDistance:
                  bestNewDistance = newDistance
                  bestAction = [action]
              elif newDistance == bestNewDistance:
                  bestAction.append(action)
          return random.choice(bestAction)
