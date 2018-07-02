# easy-blockchain

#### 三方库
```
github.com/gorilla/mux
github.com/davecgh/go-spew/spew
github.com/joho/godotenv
```

#### 主要函数
```
//生成区块
func generateBlock(oldBlock Block, BPM int) (Block, error) {
	var newBlock Block
	t := time.Now()
	newBlock.Index = oldBlock.Index + 1
	newBlock.Timestamp = t.String()
	newBlock.BPM = BPM
	newBlock.PreHash = oldBlock.Hash
	newBlock.Hash = calculateHash(newBlock)
	return newBlock, nil
}

//计算区块hash
func calculateHash(block Block) string {
	record := string(block.Index) + block.Timestamp + string(block.BPM) + block.PreHash
	h := sha256.New()
	h.Write([]byte(record))
	hashed := h.Sum(nil)
	return hex.EncodeToString(hashed)
}

//验证区块
func isBlockValid(newblock Block, oldBlock Block) bool {
	if oldBlock.Index+1 != newblock.Index {
		return false
	}
	if oldBlock.Hash != newblock.PreHash {
		return false
	}
	if calculateHash(newblock) != newblock.Hash {
		return false
	}
	return true
}
```