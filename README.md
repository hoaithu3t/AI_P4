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
          
 Bài 4:
 Khác câu 1 ở chỗ sử dụng tập mẫu để đưa ra suy diễn
 Hàm *initializeUniformly():* 

	for counter in range(self.numParticles):
		self.particles.append(legalPositions[counter % len(legalPositions)])

Hàm *getBeliefDistribution():* Khởi tạo xác suất cho mỗi vị trí trong tập particles là bằng nhau và tổng bằng 1 (phân phối đều):

    for particle in self.particles:
        beliefDistribution[particle] += 1.0 / self.numParticles

Hàm *observe():* Tương tự hàm observe() của Question 1:

    allPossible = util.Counter()
                for p in self.legalPositions:
                    trueDistance = util.manhattanDistance(p, pacmanPosition)
                    if emissionModel[trueDistance] > 0:
                        allPossible[p] = emissionModel[trueDistance] * beliefDistribution[p]
                                                        
Khởi tạo lại tập mẫu nếu như xác suất ở tất cả các vị trí đều bằng 0:

    if allPossible.totalCount() == 0:
        self.initializeUniformly(gameState)
        
Bài 5: Tương tự bài 2

