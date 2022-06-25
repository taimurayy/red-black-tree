# red-black-tree
red black tree code in c++
#pragma once
#include<iostream>
#include<fstream>
using namespace std;
struct node {
	int data;
	node *left;
	node *right;
	char color;
};
class redblack {

	
	node *root;


	node *createnode(int data)
	{
		node *nn = new node();
		nn->data = data;
		nn->left = NULL;
		nn->right = NULL;
		nn->color = 'R';
		return nn;
	}

	void LNR(node *root)
	{
		if (root != NULL)
		{
			LNR(root->left);
			cout << root->data << "," << root->color << " ";
			LNR(root->right);
		}

	}

	void NLR(node *root)
	{
		if (root != NULL)
		{
			cout << root->data << "," << root->color << " ";
			NLR(root->left);
			NLR(root->right);
		}

	}
	void LRN(node *root)
	{
		if (root != NULL)
		{
			LRN(root->left);
			LRN(root->right);
			cout << root->data << "," << root->color << " ";
		}

	}


	void NRL(node *root)
	{
		if (root != NULL)
		{

			cout << root->data << "," << root->color << " ";
			NRL(root->right);
			NRL(root->left);
		}

	}

	void RNL(node *root)
	{
		if (root != NULL)
		{

			
			RNL(root->right);
			cout << root->data << "," << root->color << " ";
			RNL(root->left);
		}

	}


	void RLN(node *root)
	{
		if (root != NULL)
		{


			RLN(root->right);
			RLN(root->left);
			cout << root->data << "," << root->color << " ";
		}

	}


	void checktree(int data)
	{
		node *gpprev = NULL;
		node *temp = root;
		node *gp = NULL;
		node *p = NULL;
		while (1)
		{
			if (data < temp->data)
			{
				if (temp->left == NULL)
				{
					break;
				}
				gpprev = gp;
				gp = p;
				p = temp;
				temp = temp->left;

			}
			else
			{
				if (temp->right == NULL)
				{
					break;
				}
				gpprev = gp;
				gp = p;
				p = temp;
				temp = temp->right;

			}
		}
		if (gp != NULL)
		{
			if (p->color == 'R'&& temp->color == 'R')
			{
				if ((gp->left == NULL||gp->left->color=='B') && gp->right == p && gp->right->right == temp)
				{
					if (root == gp)
					{
						root = leftrotation(root);
					}
					else
					{
						gpprev->right = leftrotation(gp);
					}
				}
				else if ((gp->right == NULL||gp->right->color=='B' )&& gp->left == p && gp->left->left == temp)
				{
					if (root == gp)
					{
						root = rightrotation(root);
					}
					else
					{
						gpprev->left = rightrotation(gp);
					}
				}
				else if (gp->left == p && gp->left->right == temp)
				{

					gp = leftrightrotaion(gp);
					if (root == gp)
					{
						root = rightrotation(root);
					}
					else
					{
						gpprev->left = rightrotation(gp);
					}
				}
				else if (gp->right == p && gp->right->left == temp)
				{

					gp = rightleftrotaion(gp);
					if (root == gp)
					{
						root = leftrotation(root);
					}
					else
					{
						gpprev->right = leftrotation(gp);
					}
				}
				else if (gp->left != NULL || gp->right != NULL)
				{
					gp->color = 'R';
					gp->left->color = 'B';
					gp->right->color = 'B';
				}
			}
		}


	}


	node *rightleftrotaion(node *gp)
	{

		node *p = gp->right;
		node *n = p->left;

		gp->right = n;
		n->right = p;
		p->left = NULL;



		return gp;

	}

	node *leftrightrotaion(node *gp)
	{

		node *p = gp->left;
		node *n = p->right;

		gp->left = n;
		n->left = p;
		p->right = NULL;



		return gp;

	}

	node *rightrotation(node *x)
	{
		

		node *p = x->left;

		if (p->right != NULL)
		{
			node *hehe = p->right;
			x->left = hehe;
		}
		else
		{
			x->left = NULL;
		}

		p->right = x;
		p->color = 'B';
		p->left->color = 'R';
		p->right->color = 'R';

		return p;

	}

	node *leftrotation(node *x)
	{

		node *p = x->right;

		if (p->left != NULL)
		{
			node *hehe = p->left;
			x->right = hehe;
		}
		else
		{
			x->right = NULL;
		}

		p->left = x;
		x->right = NULL;
		p->color = 'B';
		p->left->color = 'R';
		p->right->color = 'R';

		return p;

	}

	


public:

	redblack()
	{
		root = NULL;
	}
	
	bool search(int data)
	{
		node *temp = root;
		while (temp != NULL)
		{
			if (data == temp->data)
			{
				return 1;
			}
			if (data < temp->data)
			{
				temp = temp->left;
			}
			else
			{
				temp = temp->right;
			}
		}
		return 0;
	}

	void insert(int data)
	{
		
		
			if (root == NULL)
			{
				root = createnode(data);
			}
			else
			{
				node *n = createnode(data);
				node *temp = root;
			
				while (1)
				{
					if (data < temp->data)
					{
						if (temp->left == NULL)
						{
							temp->left = createnode(data);
							break;
						}
						temp = temp->left;
					}
					else
					{
						if (temp->right == NULL)
						{
							temp->right = createnode(data);
							break;
						}
						temp = temp->right;
					}
				}
			}


			checktree(data);

			root->color = 'B';
		
	}

	void DESTROYTREE()
	{
		delete root;
		root = NULL;
	}

	void displayparent(int data)
	{
		node *temp = root;
		while (temp != NULL)
		{
			if (data < temp->data)
			{
				if (data == temp->left->data)
				{
					cout << temp->data << " ";
					break;
				}
				temp = temp->left;
			}
			else
			{
				if (data == temp->right->data)
				{
					cout << temp->data << " ";
					break;
				}
				temp = temp->right;
			}
		}

	}

	void print()
	{

		//cout << "RIGHT LEFT NODE: "; RLN(root); cout << endl;
		//cout << "NODE RIGHT LEFT: "; NRL(root); cout << endl;
		//cout << "RIGHT NODE LEFT: "; RNL(root); cout << endl;
		//cout << "LEFT NODE RIGHT: "; LNR(root); cout << endl;
		//cout << "LEFT RIGHT NODE: "; LRN(root); cout << endl;
		//cout << "NODE LEFT RIGHT: "; 
		NLR(root); cout << endl;
	}

}; 
void readinput(redblack&obj)
{

	ifstream fin;
	fin.open("input.txt");

	while (fin.good())
	{
		int n = 0;
		fin >> n;
		obj.insert(n);
	}





}
