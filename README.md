# async-await
async 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里，最后返回Promise。
下面给出spawn函数的实现
function tcf(genF) {
  const gen = genF();
  return function next(v) {
    return new Promise((resolve, reject) => {
      try {
        next = gen.next(v);
        if(next.done) return resolve(next.value);
        return Promise.resolve(next.value).then(next).then(resolve, reject);
      } catch (error) {
        reject(error);
      }
    })
  }
}
